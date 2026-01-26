---
project_name: 'convenient-store'
user_name: 'Shaun'
date: '2026-01-26'
status: 'complete'
sections_completed: ['technology_stack', 'domain_rules', 'data_access', 'api_errors', 'frontend', 'backend', 'naming', 'testing', 'anti_patterns']
---

# Project Context for AI Agents

_Critical rules and patterns for implementing code in the convenient-store POS system. Focus on unobvious details that agents might otherwise miss._

---

## Technology Stack & Versions

### Frontend
- **Next.js 14+** (App Router, NOT Pages Router)
- **TypeScript** (strict mode)
- **Tailwind CSS** + **shadcn/ui**
- **Zustand** (client state) + **TanStack Query** (server state)
- **React Hook Form** + **Zod** (forms/validation)
- **Orval** (API client generation from OpenAPI)

### Backend
- **Django 5+** + **Django REST Framework**
- **DRF Spectacular** (OpenAPI spec generation)
- **Python 3.11+**
- **PostgreSQL 15+**

### Development
- **Docker Compose** (PostgreSQL container)
- **Ruff** (Python linting/formatting)
- **ESLint + Prettier** (TypeScript/JS)

---

## Critical Domain Rules

### FIFO Enforcement (NON-NEGOTIABLE)
- ALL inventory deductions MUST use FIFO (First-In-First-Out)
- Deduct from oldest batch first based on `created_at`
- NEVER allow bypass of FIFO — this is a core domain invariant
- Expired batches are skipped (not sold, auto-wasted)

### Backflushing (Invisible Inventory Deduction)
- When a Sales Item is sold, its Recipe explodes into Physical Item deductions
- Deduction happens automatically at checkout — staff sees nothing
- Each ingredient deducts from FIFO batches independently
- If ANY ingredient is insufficient, checkout FAILS (no partial sale)

### Physical SKU vs Sales SKU Separation
- **Physical Items**: What's in inventory (Bánh mì, Chả lụa, etc.)
- **Sales Items**: What's on the menu (Bánh mì Chả lụa combo)
- One Physical Item can appear in multiple Sales Items (via recipes)
- Inventory tracks Physical Items; Revenue tracks Sales Items

### Batch Expiry (Hour-Level Precision)
- Each batch has `expires_at` timestamp (hour-level, not just date)
- State conversion (Frozen → Steamed) creates NEW batch with NEW expiry
- Expired batches trigger alerts, then auto-waste if not acknowledged

### Audit Trail (100% Traceability)
- EVERY inventory movement must be logged with:
  - `staff_id` (who)
  - `timestamp` (when)
  - `reason_code` (why: sale, waste, adjustment, conversion)
  - `batch_id` (which batch affected)
- Voided transactions are RETAINED (soft delete), never hard deleted
- Any number in a report must drill down to contributing transactions

---

## Data Access Rules

### Hybrid ORM/Raw SQL Pattern

**Use Django ORM for:**
- Simple CRUD (list items, create staff, get order by ID)
- Basic filters and lookups
- Any query without complex locking or multi-row updates

**Use Raw SQL (in `queries.py` files) for:**
- FIFO batch deduction (requires `FOR UPDATE` locking)
- Backflushing (multi-table deduction in single transaction)
- Batch expiry checks (complex date/time logic)
- Variance reports (window functions, CTEs)
- Any query needing transaction isolation guarantees

### Raw SQL File Locations
```
backend/inventory/queries.py   # FIFO, batch management, expiry
backend/reports/queries.py     # Variance, drill-down, aggregations
backend/pos/queries.py         # Complex checkout (if needed)
```

### Raw SQL Pattern
```python
from django.db import connection

def deduct_fifo_batches(physical_item_id: int, quantity: int, store_id: int):
    with connection.cursor() as cursor:
        cursor.execute("""
            SELECT id, quantity_remaining
            FROM batches
            WHERE physical_item_id = %s 
              AND store_id = %s 
              AND quantity_remaining > 0
              AND expires_at > NOW()
            ORDER BY created_at ASC
            FOR UPDATE
        """, [physical_item_id, store_id])
        # ... FIFO deduction logic
```

### Service Layer Pattern
```python
from catalog.models import Recipe, SalesItem  # ORM for simple reads
from inventory.queries import deduct_fifo_batches  # Raw SQL for complex

def execute_backflush(order_id: int, store_id: int):
    order = Order.objects.get(id=order_id)  # ORM
    for item in order.items.all():          # ORM
        recipe = Recipe.objects.filter(sales_item_id=item.item_id).first()
        if recipe:
            for ingredient in recipe.ingredients.all():
                deduct_fifo_batches(...)    # Raw SQL
```

---

## API & Error Handling Rules

### API Endpoint Patterns
- Resource endpoints: Plural nouns (`/api/items/`, `/api/orders/`)
- Action endpoints: Domain prefix + verb (`/api/pos/checkout`, `/api/inventory/convert`)
- URL style: `kebab-case` (`/api/order-items/`)
- Query/path params: `snake_case` (`?store_id=1`, `/api/items/{item_id}/`)

### Error Response Format (ALWAYS)
```json
{
  "error": {
    "code": "INSUFFICIENT_STOCK",
    "message": "Không đủ tồn kho cho Chả cá",
    "details": {
      "item_id": 42,
      "requested": 5,
      "available": 2
    }
  }
}
```

### Standard Error Codes
| Code | When to Use |
|------|-------------|
| `INSUFFICIENT_STOCK` | Not enough inventory for sale |
| `BATCH_EXPIRED` | Batch has expired, cannot sell |
| `ITEM_INACTIVE` | Item deactivated |
| `INVALID_PIN` | PIN authentication failed |
| `PERMISSION_DENIED` | Role lacks permission |
| `VALIDATION_ERROR` | Request validation failed |
| `COMBO_UNAVAILABLE` | Combo ingredients not available |

### Frontend Error Handling
- **Action errors (checkout, save)**: Show toast notification (non-blocking)
- **Form validation errors**: Show inline under field (red text)
- **Page errors**: Use Next.js error boundary (`error.tsx`)

```typescript
const checkout = useCheckout({
  onError: (error) => {
    toast.error(error.response?.data?.error?.message || 'Checkout failed');
  },
  onSuccess: () => {
    toast.success('Đơn hàng hoàn tất!');
    useCartStore.getState().clearCart();
  },
});
```

---

## Frontend Architecture Rules

### State Management (Strict Separation)
- **Zustand**: Client-only state (cart, UI, current auth session)
- **TanStack Query**: ALL server data (items, orders, alerts, reports)
- NEVER store server data in Zustand — let Query cache it

### Zustand Store Pattern
```typescript
export const useCartStore = create<CartState>((set, get) => ({
  items: [],
  addItem: (item) => set((state) => ({ items: [...state.items, item] })),
  clearCart: () => set({ items: [] }),
  getTotal: () => get().items.reduce((sum, i) => sum + i.price * i.quantity, 0),
}));
```

### Component Organization
```
src/components/ui/          # shadcn components (auto-generated)
src/components/common/      # Shared custom components
src/app/(pos)/components/   # POS-specific components
src/app/(admin)/components/ # Admin-specific components
```

### Component Rules
- Use **named exports** (NOT default exports)
- File naming: `kebab-case.tsx` (`cart-item.tsx`)
- Component naming: `PascalCase` (`CartItem`)
- Props interface: `{ComponentName}Props`

```typescript
// CORRECT
export function CartItem({ item }: CartItemProps) { ... }

// WRONG
export default function CartItem() { ... }
```

### Route Group Layouts
| Route Group | Purpose | Layout Style |
|-------------|---------|--------------|
| `(pos)/` | POS Mode | Full-screen, no nav, touch-optimized |
| `(staff)/` | Staff Tasks | Task-focused, minimal nav |
| `(admin)/` | Admin Dashboard | Sidebar navigation |
| `(display)/` | Customer Display | View-only, no interaction |

### Loading States
- **Page loading**: Use `loading.tsx` with skeleton loaders
- **Button actions**: Disabled + spinner inside button
- **POS Mode exception**: Minimize loading UI (speed critical)

### Touch-First UI (POS Mode)
- Minimum touch target: **44px × 44px**
- No hover-only interactions
- Large, clear buttons
- Minimal taps to complete transaction (target: <10 seconds)

---

## Backend Architecture Rules

### Django App Organization
```
backend/
├── auth/          # Staff, PIN, JWT, permissions
├── catalog/       # PhysicalItem, SalesItem, Recipe, Combo
├── inventory/     # Batch, InventoryMovement, FIFO, backflush
├── pos/           # Order, OrderItem, Payment, checkout
├── alerts/        # Alert generation, acknowledgment
├── reports/       # Variance, sales, inventory reports
├── shifts/        # Shift, CashFloat, CashDrop
├── stores/        # Store config, PaymentMethod, PrinterConfig
└── common/        # Shared middleware, exceptions, utils
```

### Service Layer Pattern
- **Views**: HTTP request/response ONLY — no business logic
- **Services**: Business logic lives here (`services/*.py`)
- **Queries**: Raw SQL lives here (`queries.py`)
- **Models**: Schema definition + simple ORM queries

```python
# CORRECT: View delegates to service
class CheckoutView(APIView):
    def post(self, request):
        result = checkout_service.process_checkout(request.data, request.user)
        return Response(result)

# WRONG: Business logic in view
class CheckoutView(APIView):
    def post(self, request):
        order = Order.objects.create(...)
        for item in items:
            # 50 lines of checkout logic...
```

### JWT Authentication
```
Authorization: Bearer <access_token>

Token payload:
{
  "staff_id": 5,
  "role": "staff",  // or "manager"
  "store_id": 1,
  "exp": 1234567890
}
```

### Permission Classes
```python
class IsStaff(BasePermission):
    """Any authenticated staff member"""

class IsManager(BasePermission):
    """Manager role only"""
```

### Multi-Store Ready
- ALL tenant-scoped tables MUST have `store_id` column
- ALL queries MUST filter by `store_id`
- Even though MVP is single-store, don't skip this

---

## Naming Conventions

### Database
| Element | Pattern | Example |
|---------|---------|---------|
| Tables | `snake_case` plural | `order_items`, `physical_items` |
| Columns | `snake_case` | `created_at`, `unit_price` |
| Primary key | `id` | `id SERIAL PRIMARY KEY` |
| Foreign key | `{table}_id` | `staff_id`, `order_id` |
| Indexes | `idx_{table}_{columns}` | `idx_orders_created_at` |

### API (JSON)
| Element | Pattern | Example |
|---------|---------|---------|
| Field names | `snake_case` | `staff_id`, `created_at` |
| Dates | ISO 8601 | `"2026-01-26T14:30:00Z"` |
| Money | Integer (đồng) | `25000` (= 25,000₫) |
| IDs | Integer | `42` |
| Booleans | `true`/`false` | `"is_active": true` |
| Nulls | Explicit | `"voided_at": null` |

### Frontend (TypeScript)
| Element | Pattern | Example |
|---------|---------|---------|
| Files | `kebab-case` | `cart-item.tsx`, `format-currency.ts` |
| Components | `PascalCase` | `CartItem`, `PriceDisplay` |
| Functions/variables | `camelCase` | `getCartTotal`, `isExpired` |
| Types/interfaces | `PascalCase` | `OrderItem`, `CartState` |
| Constants | `SCREAMING_SNAKE` | `MAX_CART_ITEMS` |
| Zustand stores | `use{Name}Store` | `useCartStore` |

### Backend (Python)
| Element | Pattern | Example |
|---------|---------|---------|
| Files | `snake_case` | `checkout_service.py` |
| Classes | `PascalCase` | `CheckoutService` |
| Functions/variables | `snake_case` | `get_cart_total` |
| Constants | `SCREAMING_SNAKE` | `MAX_CART_ITEMS` |

---

## Testing Rules

### Test Priority
| Priority | Area | Rationale |
|----------|------|-----------|
| **HIGH** | Inventory engine (FIFO, backflush, conversion) | Core differentiator, accuracy = money |
| **HIGH** | Checkout flow | Financial transactions |
| **MEDIUM** | Auth flow | Security |
| **LOW** | UI components | Demo only, manual test sufficient |

### Test Organization
- **Frontend**: Co-located (`cart.test.ts` next to `cart.ts`)
- **Backend**: Django convention (`app/tests/test_*.py`)

### Test Structure
```typescript
// Frontend
describe('useCartStore', () => {
  it('adds item to cart', () => { ... });
  it('calculates total correctly', () => { ... });
});
```

```python
# Backend
class FIFODeductionTest(TestCase):
    def test_deducts_from_oldest_batch_first(self):
        ...
    def test_skips_expired_batches(self):
        ...
```

### Critical Test Cases (MUST HAVE)
```python
def test_fifo_deducts_oldest_batch_first()
def test_fifo_skips_expired_batches()
def test_fifo_fails_if_insufficient_total_stock()
def test_backflush_deducts_all_recipe_ingredients()
def test_backflush_fails_if_any_ingredient_insufficient()
def test_conversion_creates_new_batch_with_new_expiry()
def test_conversion_deducts_from_source_batch()
```

---

## Critical Don't-Miss Rules

### NEVER Do These

**Inventory:**
- ❌ NEVER bypass FIFO — no "pick specific batch" option
- ❌ NEVER allow selling from expired batches
- ❌ NEVER hard-delete inventory movements — always soft delete with audit
- ❌ NEVER modify orders after creation — void and recreate instead

**Data Access:**
- ❌ NEVER use Django ORM for FIFO deduction — use raw SQL with `FOR UPDATE`
- ❌ NEVER query without `store_id` filter on tenant-scoped tables
- ❌ NEVER put business logic in views — use service layer

**Frontend:**
- ❌ NEVER use default exports — always named exports
- ❌ NEVER store server data in Zustand — use TanStack Query
- ❌ NEVER use hover-only interactions in POS mode
- ❌ NEVER block POS UI with loading spinners (use optimistic updates)

**API:**
- ❌ NEVER return plain text errors — always use structured `{error: {code, message, details}}`
- ❌ NEVER expose stack traces in API responses

### ALWAYS Do These

**Inventory:**
- ✅ ALWAYS log inventory movements with `staff_id`, `timestamp`, `reason_code`
- ✅ ALWAYS check batch expiry before any deduction
- ✅ ALWAYS use transactions for multi-table updates

**Data:**
- ✅ ALWAYS include `store_id` on new tables (multi-store ready)
- ✅ ALWAYS use `snake_case` for database columns and API fields

**Frontend:**
- ✅ ALWAYS use 44px minimum touch targets in POS mode
- ✅ ALWAYS show Vietnamese text for user-facing messages
- ✅ ALWAYS handle loading and error states

**Security:**
- ✅ ALWAYS hash PINs with `make_password`
- ✅ ALWAYS validate JWT on every protected endpoint
- ✅ ALWAYS check role permissions before sensitive operations

### Edge Cases to Handle

```python
# Partial batch deduction
# If batch has 3 units, but we need 5:
# - Take all 3 from first batch
# - Take remaining 2 from next FIFO batch

# Exact expiry boundary
# If batch expires at 14:00 and current time is 14:00:
# - Treat as EXPIRED (>=, not >)

# Zero quantity batches
# After full deduction, batch.quantity_remaining = 0:
# - Keep batch record (for audit trail)
# - Exclude from future FIFO queries (WHERE quantity_remaining > 0)

# Combo detection with insufficient ingredients
# If combo would be cheaper but one ingredient is out:
# - Fall back to individual items
# - Show suggestion but don't force combo
```

---

_Last updated: 2026-01-26_
