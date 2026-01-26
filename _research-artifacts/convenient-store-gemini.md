# **BÁO CÁO NGHIÊN CỨU CHUYÊN SÂU: TỐI ƯU HÓA HỆ THỐNG QUẢN LÝ CỬA HÀNG TIỆN LỢI TẠI VIỆT NAM \- TỪ MÔ HÌNH HỌC THUẬT ĐẾN THỰC TIỄN DOANH NGHIỆP**

## **1\. Tổng quan Nghiên cứu và Phạm vi Báo cáo**

### **1.1. Bối cảnh Thị trường Cửa hàng Tiện lợi (CVS) tại Việt Nam**

Thị trường bán lẻ hiện đại tại Việt Nam đang chứng kiến sự chuyển dịch mạnh mẽ từ các mô hình tạp hóa truyền thống sang chuỗi cửa hàng tiện lợi (Convenience Store \- CVS) hoạt động 24/7. Sự hiện diện của các "gã khổng lồ" quốc tế như 7-Eleven, Circle K, FamilyMart, và GS25 đã định hình lại thói quen tiêu dùng của người Việt, đặc biệt là giới trẻ (Gen Z và Millennials). Không còn đơn thuần là nơi mua sắm các nhu yếu phẩm đóng gói (FMCG), các chuỗi CVS tại Việt Nam đã tiến hóa thành một mô hình lai ghép phức tạp: "Retail \+ Fast Food Restaurant" (Bán lẻ kết hợp Nhà hàng phục vụ nhanh).1

Trong hệ sinh thái này, nhóm hàng "Đồ ăn chế biến" (Made-to-Order/Fresh Food) đóng vai trò là "nam châm" thu hút lưu lượng khách hàng (traffic driver) và mang lại biên lợi nhuận gộp cao hơn nhiều so với hàng tiêu dùng thông thường. Các món ăn đặc trưng như mì trộn (Circle K), lẩu ly, bánh mì tươi, hay cơm nắm (7-Eleven) đòi hỏi một quy trình vận hành cực kỳ phức tạp, nằm giữa ranh giới của quản lý kho bán lẻ (SKU-based) và quản lý bếp nhà hàng (Recipe-based).

### **1.2. Mục tiêu Báo cáo**

Báo cáo này được xây dựng nhằm thực hiện hai nhiệm vụ cốt lõi theo yêu cầu của đơn vị:

1. **Phân tích phản biện (Critical Analysis):** Đánh giá sâu sắc báo cáo giữa kỳ "Hệ thống quản lý cửa hàng tiện lợi" (tài liệu 'main 1.pdf' \- sau đây gọi là "Báo cáo Cơ sở").2 Mục tiêu là bóc tách các giả định học thuật, đối chiếu với thực tế vận hành khốc liệt của thị trường, từ đó chỉ ra các "lỗ hổng nghiệp vụ" (Operational Gaps) nghiêm trọng có thể dẫn đến thất thoát tài chính hoặc vỡ trận vận hành.  
2. **Đặc tả giải pháp (System Specification):** Cung cấp một bản thiết kế hệ thống phần mềm hoàn chỉnh, tập trung vào phân hệ "Quản lý Chế biến & Bếp" (Production & Kitchen Management). Giải pháp này sẽ tích hợp các tiêu chuẩn vận hành (SOP) từ 7-Eleven và Circle K, sử dụng công nghệ Hiển thị Bếp (KDS) và quản lý kho định lượng (Inventory Recipe Explosion) để giải quyết triệt để các vấn đề đã nêu.

### **1.3. Phương pháp Tiếp cận**

Báo cáo sử dụng phương pháp phân tích so sánh (Comparative Analysis) kết hợp với tư duy thiết kế hệ thống hướng dịch vụ (Service-Oriented Architecture). Dữ liệu được tổng hợp từ tài liệu nội bộ sinh viên 2 và các nguồn dữ liệu mở về quy trình vận hành của 7-Eleven 1, Circle K 5, cùng các tiêu chuẩn an toàn thực phẩm quốc tế.7

## ---

**2\. Phân tích Hiện trạng Hệ thống từ Báo cáo Cơ sở ('main 1.pdf')**

Phần này sẽ đi sâu vào cấu trúc, luồng dữ liệu và giả định nghiệp vụ của nhóm tác giả trong tài liệu 'main 1.pdf' 2, đặc biệt tập trung vào cách tiếp cận đối với nhóm hàng chế biến.

### **2.1. Cấu trúc Hệ thống Đề xuất trong Báo cáo Cơ sở**

Theo tài liệu 2, hệ thống được thiết kế hướng tới việc quản lý nội bộ một cửa hàng đơn lẻ với các module chức năng chính:

* **Quản lý Danh mục (Catalog Management):** Sản phẩm, nhân viên, khách hàng.  
* **Quản lý Bán hàng (Sales Management):** Xử lý hóa đơn, thanh toán, và khuyến mại cơ bản.  
* **Quản lý Kho (Inventory Management):** Nhập, xuất, kiểm kê, quản lý lô (Batch).  
* **Quản lý Nhân sự & Báo cáo.**

Điểm đáng ghi nhận là nhóm tác giả đã nhận diện được sự tồn tại của nghiệp vụ chế biến thông qua chức năng "2.2 Quản lý đồ ăn chế biến" 2 và mô tả sơ đồ luồng dữ liệu (DFD) Mức 2 cho quy trình này.2 Tuy nhiên, cách triển khai chi tiết lại bộc lộ tư duy của một hệ thống bán lẻ truyền thống (như siêu thị) đang cố gắng "ép" quy trình nhà hàng vào khuôn khổ quản lý mã vạch đơn thuần.

### **2.2. Phân tích Luồng Nghiệp vụ Chế biến (The Processing Workflow)**

Dựa trên mô tả tại trang 8 và sơ đồ DFD tại trang 14-15 của 'main 1.pdf' 2, quy trình xử lý đồ ăn chế biến hiện tại được mô hình hóa theo trình tự tuyến tính đồng bộ:

1. **Tiếp nhận yêu cầu:** Nhân viên bán hàng nhận thông tin món ăn từ khách.  
2. **Kiểm tra tồn kho nguyên liệu (Synchronous Check):** Hệ thống truy vấn kho để xem có đủ nguyên liệu (ví dụ: xúc xích, mì, trứng) hay không.  
3. **Xác nhận & Thanh toán:** Nếu kho đủ, đơn hàng được tạo và khách thanh toán.  
4. **Chế biến:** Nhân viên thực hiện làm món ăn.  
5. **Trừ kho:** Hệ thống trừ số lượng nguyên liệu tương ứng.

#### **2.2.1. Lỗ hổng "Nút thắt Đồng bộ" (The Synchronous Bottleneck)**

Vấn đề chí mạng trong thiết kế này nằm ở bước "Kiểm tra tồn kho" diễn ra *trước* khi thanh toán.2

* **Thực tế:** Trong giờ cao điểm (rush hour) tại Circle K, tốc độ giao dịch (transaction velocity) là yếu tố sống còn. Một nhân viên phải xử lý 30-45 giây/khách. Việc hệ thống phải "dừng" lại để truy vấn database, tính toán tổng lượng tồn của từng thành phần (ví dụ: 1 tô mì cần kiểm tra tồn của: vắt mì, gói súp, trứng, rau, ly giấy, nĩa) sẽ tạo ra độ trễ (latency) lớn.  
* **Rủi ro:** Nếu dữ liệu kho bị lệch (data drift) \- điều thường xuyên xảy ra do rơi vãi, hư hỏng chưa kịp báo \- hệ thống sẽ báo "Hết hàng" và chặn nhân viên bán, dù thực tế nguyên liệu vẫn còn trên kệ bếp. Điều này gây thất thu trực tiếp (lost sales) và ức chế cho khách hàng.

#### **2.2.2. Sự thiếu vắng Khái niệm "Công thức Động" (Dynamic Recipes)**

Mô hình dữ liệu ERD tại trang 31 2 cho thấy bảng Recipe liên kết trực tiếp với Product. Cấu trúc này gợi ý mối quan hệ 1-1 hoặc 1-n đơn giản: Một món ăn có một danh sách nguyên liệu cố định.

* **Thiếu sót:** Thực tế tại Circle K hay 7-Eleven, khách hàng luôn có nhu cầu tùy biến (Customization): "Mì trộn không lấy rau", "Thêm 2 trứng", "Nhiều sốt cay".  
* **Hệ quả:** Với thiết kế hiện tại, nhân viên không thể nhập liệu chính xác các yêu cầu này vào hệ thống. Họ buộc phải ghi chú thủ công hoặc "nói miệng" với bộ phận bếp. Hậu quả là kho sẽ bị trừ sai (hệ thống trừ rau dù khách không lấy), dẫn đến lệch kho (inventory variance) ngày càng lớn theo thời gian.

#### **2.2.3. Quy trình "Kho kiêm Bếp" bất hợp lý**

Trong phân công vai trò 2, "Thủ kho" được giao trách nhiệm "Quản lý đồ ăn chế biến". Đây là một sự nhầm lẫn nghiêm trọng về vận hành.

* **Thực tế:** Thủ kho (Warehouse Keeper) làm việc trong kho lưu trữ (Backroom), quản lý nhập hàng và các thùng carton lớn. Người thực hiện chế biến mì trộn, nướng xúc xích là nhân viên bán hàng (Sales Staff) hoặc nhân viên bếp chuyên biệt (Kitchen Staff) tại khu vực quầy (Front Counter).  
* **Hệ quả:** Việc gán quyền hạn và trách nhiệm sai vai trò trong phần mềm sẽ dẫn đến việc nhân viên bán hàng không có quyền truy cập vào các chức năng cần thiết để báo cáo hư hỏng hoặc điều chỉnh trạng thái chế biến, hoặc ngược lại, thủ kho bị quá tải với các nghiệp vụ không thuộc chuyên môn.

### **2.3. Hạn chế trong Quản lý Lô (Batch Management)**

Báo cáo đề cao tính năng "Quản lý lô hàng" để theo dõi hạn sử dụng (FIFO).2 Tuy nhiên, áp dụng logic này cho bếp chế biến là không khả thi.

* **Kịch bản:** Khi nhân viên bếp lấy 1 quả trứng từ rổ để luộc, họ không thể biết quả trứng đó thuộc "Lô nhập ngày 01/10" hay "Lô nhập ngày 02/10". Họ chỉ lấy quả trứng thuận tiện nhất.  
* **Bất cập:** Yêu cầu nhân viên chọn chính xác lô hàng trên phần mềm mỗi khi bán một tô mì là phi thực tế, làm chậm thao tác và dẫn đến việc nhân viên chọn đại một lô cho xong, làm sai lệch dữ liệu hạn sử dụng (Expiry tracking) của hệ thống.

## ---

**3\. Nghiên cứu Chuyên sâu Nghiệp vụ Thực tế (7-Eleven & Circle K Việt Nam)**

Để khắc phục các hạn chế trên, chúng ta cần thấu hiểu "cỗ máy" vận hành thực sự đằng sau các chuỗi CVS hàng đầu.

### **3.1. Mô hình "Tanpin Kanri" và Hệ sinh thái Thực phẩm Tươi của 7-Eleven**

7-Eleven không chỉ là cửa hàng tiện lợi, họ là một công ty công nghệ logistics bán thực phẩm. Triết lý "Tanpin Kanri" (Quản lý từng món đơn lẻ) là xương sống của họ.1

#### **3.1.1. Dự báo Nhu cầu (Demand Forecasting)**

Thay vì chỉ nhập hàng khi kho hết (Reorder Point), hệ thống của 7-Eleven dự báo nhu cầu tiêu thụ dựa trên dữ liệu lịch sử, thời tiết, sự kiện địa phương (ví dụ: có trận bóng đá gần đó) để gợi ý số lượng đặt hàng cho các món tươi sống (Cơm nắm, Sandwich) vốn có hạn sử dụng chỉ 24-48 giờ.4

* **Bài học:** Hệ thống cần module "Gợi ý đặt hàng" (Suggested Ordering) dựa trên thuật toán, không chỉ là các phiếu nhập tay thủ công như Báo cáo Cơ sở đề xuất.

#### **3.1.2. Quản lý Hạn sử dụng theo Giờ (Time-based Expiry)**

Khác với hàng khô quản lý theo ngày (Date-based), đồ ăn chế biến tại 7-Eleven được quản lý theo giờ (Time-based). Một chiếc bánh mì kẹp thịt có hạn sử dụng đến "14:00 ngày 22/05". Nếu đến 14:01, mã vạch của sản phẩm đó sẽ bị vô hiệu hóa tại POS, ngăn chặn việc bán hàng hết hạn.3

* **Lỗ hổng Báo cáo:** Báo cáo Cơ sở chỉ đề cập đến hạn sử dụng ngày, hoàn toàn bỏ qua yếu tố kiểm soát thời gian thực này, gây rủi ro an toàn thực phẩm nghiêm trọng.

### **3.2. Quy trình "Hot Kitchen" tại Circle K Việt Nam**

Circle K thành công nhờ định vị là nơi "Ăn \- Uống \- Ngồi lại". Quy trình của họ được tối ưu cho tốc độ và sự đơn giản hóa.5

#### **3.2.1. Phân tách Luồng Order và Fulfillment (Decoupling)**

Khác với quy trình tuyến tính trong Báo cáo Cơ sở, Circle K áp dụng mô hình bất đồng bộ:

1. **Order & Pay:** Khách hàng chọn món, tùy chỉnh (cay/không cay), thanh toán và nhận phiếu số thứ tự (Token/Receipt).  
2. **Routing:** Thông tin đơn hàng được chuyển ngay lập tức vào khu vực chế biến (bằng máy in bếp hoặc màn hình KDS).  
3. **Preparation:** Nhân viên bếp chế biến độc lập với nhân viên thu ngân.  
4. **Handover:** Gọi số và giao món.

#### **3.2.2. Xử lý Nguyên liệu Bán thành phẩm (Semi-finished Goods)**

Để đảm bảo tốc độ, Circle K không chế biến từ nguyên liệu thô (Raw) hoàn toàn. Ví dụ: Trứng được luộc sẵn và bóc vỏ vào đầu ca (Batch cooking). Xúc xích được nướng sơ.

* **Yêu cầu hệ thống:** Phần mềm cần hỗ trợ quy trình "Chuyển đổi trạng thái" (Conversion): Từ 100 quả trứng sống (Kho Nguyên liệu) \-\> 100 quả trứng luộc (Kho Bán thành phẩm) với hạn sử dụng mới (24h). Báo cáo Cơ sở chưa có khái niệm này.

### **3.3. Vai trò của Hệ thống Hiển thị Bếp (KDS \- Kitchen Display System)**

Các nghiên cứu hiện đại 9 chỉ ra rằng KDS là trái tim của vận hành Fast Food.

* **Routing thông minh:** Món nước (Trà sữa) tự động hiện ở màn hình quầy Bar. Món ăn (Mì trộn) hiện ở màn hình Bếp nóng.  
* **Cảnh báo độ trễ (Meal Pacing):** Màn hình đổi màu (Xanh \-\> Vàng \-\> Đỏ) nếu đơn hàng quá 5 phút chưa xong, giúp quản lý giám sát chất lượng dịch vụ (Service Level Agreement \- SLA).

## ---

**4\. Phân tích Khoảng cách (Gap Analysis) và Đối chiếu Chi tiết**

Bảng dưới đây tổng hợp các thiếu sót cụ thể của Báo cáo 'main 1.pdf' so với yêu cầu thực tế, làm cơ sở cho phần đặc tả kỹ thuật sau này.

| Khía cạnh Nghiệp vụ | Giải pháp trong Báo cáo 'main 1.pdf' | Thực tế Yêu cầu tại Circle K / 7-Eleven | Lỗ hổng (Gap) & Rủi ro |
| :---- | :---- | :---- | :---- |
| **Quy trình Bán hàng** | Đồng bộ (Synchronous): Order \-\> Chờ check kho \-\> Thanh toán. | Bất đồng bộ (Asynchronous): Order \-\> Thanh toán \-\> Đẩy xuống bếp \-\> Trừ kho sau (Backflush). | **Tắc nghẽn:** Khách phải chờ hệ thống xử lý. Nếu kho lệch số liệu, không thể bán hàng dù có hàng thật. |
| **Cấu trúc Món ăn** | Đơn giản: 1 Món \= N Nguyên liệu cố định. | Phức tạp: Món chính \+ Tùy chọn (Modifiers) \+ Ghi chú (Notes). | **Sai lệch kho:** Không trừ được nguyên liệu thêm (vd: thêm phô mai). **Sai order:** Bếp làm sai yêu cầu khách. |
| **Quản lý Hạn sử dụng** | Theo Ngày (Date) & Thủ công chọn Lô. | Theo Giờ (Time) & Tự động (FEFO \- First Expired First Out). | **Rủi ro ATTP:** Bán hàng hết hạn trong ngày (vd: cơm nắm). Nhân viên tốn thời gian chọn lô. |
| **Giao tiếp Bếp \- Thu ngân** | Không có (Giả định nói miệng hoặc phiếu giấy). | Kỹ thuật số: Hệ thống KDS hoặc Máy in nhiệt tự động tách món. | **Thất lạc đơn:** Bếp quên làm món. Khách chờ lâu, khiếu nại. Không đo lường được thời gian chế biến. |
| **Quản lý Nguyên liệu** | Chỉ quản lý nhập/xuất nguyên liệu thô. | Quản lý đa cấp: Thô \-\> Sơ chế (Thawed/Cooked) \-\> Thành phẩm. | **Mù mờ tồn kho:** Không biết có bao nhiêu trứng *đã luộc* sẵn sàng bán, bao nhiêu trứng còn sống. |
| **Phân quyền** | Thủ kho quản lý chế biến. | Nhân viên Bếp/Đa năng thực hiện. Hệ thống phân quyền theo trạm làm việc (Station). | **Xung đột quy trình:** Người làm thực tế không có quyền thao tác trên phần mềm. |

## ---

**5\. Đặc tả Kỹ thuật Hệ thống Mới: Module Quản lý Chế biến & Bếp Tích hợp (IFFP)**

Dựa trên phân tích trên, phần này cung cấp bản đặc tả chi tiết (SRS) cho module "Integrated Fresh Food Processing" (IFFP), nhằm vá các lỗ hổng và nâng cấp hệ thống sinh viên lên chuẩn doanh nghiệp.

### **5.1. Kiến trúc Quy trình Nghiệp vụ (To-Be Business Process)**

Chúng ta sẽ chuyển đổi từ mô hình "Pull" (Kéo tồn kho trước khi bán) sang mô hình "Event-Driven Push" (Đẩy sự kiện đơn hàng).

#### **5.1.1. Lưu đồ Quy trình (Process Flow)**

1. **Khởi tạo Đơn hàng (POS):**  
   * Nhân viên chọn món "Mì Trộn Indomie".  
   * **Giao diện Pop-up Modifiers:** Hệ thống bắt buộc chọn "Cấp độ cay" (0-100%) và gợi ý "Thêm Topping" (Xúc xích \+10k, Phô mai \+5k).  
   * Hệ thống ghi nhận cấu trúc dữ liệu JSON chi tiết của món.  
   * Thanh toán thành công \-\> Sinh mã sự kiện ORDER\_CREATED.  
2. **Định tuyến Thông minh (Smart Routing):**  
   * Hệ thống phân tích đơn hàng:  
     * Item "Mì Trộn" \-\> Gửi lệnh in/hiển thị tới **KDS Bếp Nóng**.  
     * Item "Trà Đào" \-\> Gửi lệnh tới **KDS Quầy Bar**.  
     * Item "Snack" \-\> Không gửi KDS, khách tự lấy.  
3. **Chế biến & Theo dõi (KDS Execution):**  
   * Màn hình KDS hiển thị thẻ đơn hàng với đồng hồ đếm ngược.  
   * Quy tắc An toàn 7: Hệ thống hiển thị cảnh báo "Rửa tay" trên màn hình KDS mỗi 30 phút để nhắc nhở nhân viên.  
   * Nhân viên hoàn tất \-\> Nhấn "BUMP" (Hoàn thành) trên màn hình.  
4. **Trừ kho Tự động (Inventory Backflushing):**  
   * Ngay khi đơn hàng được tạo (hoặc khi bếp xác nhận xong), hệ thống kích hoạt thuật toán trừ kho ngầm.  
   * **Thuật toán FEFO:** Tự động tìm các lô nguyên liệu (Batch) có hạn sử dụng gần nhất để trừ, không cần nhân viên chọn.

### **5.2. Đặc tả Vai trò Người dùng (User Roles)**

Hệ thống phân quyền (RBAC) cần được mở rộng:

| Vai trò | Trách nhiệm Mới | Quyền hạn Hệ thống |
| :---- | :---- | :---- |
| **Kitchen Staff (Nhân viên Bếp)** | Tiếp nhận đơn từ KDS, chế biến, báo cáo hủy món (Waste), chuyển trạng thái nguyên liệu (Sơ chế). | Xem KDS, Thao tác "Sơ chế/Chuyển kho", Báo cáo Hủy (cần duyệt). Không có quyền xem Doanh thu. |
| **Expediter (Điều phối)** | (Dành cho cửa hàng lớn) Kiểm tra món ăn trước khi giao, ghép các món từ Bếp và Bar thành đơn hoàn chỉnh. | Xem toàn bộ KDS, Đổi trạng thái đơn hàng, In lại bill. |
| **Store Manager (Quản lý)** | Cấu hình công thức (Recipe), định nghĩa Modifiers, duyệt báo cáo hủy, xem báo cáo hiệu suất bếp (thời gian trung bình/món). | Admin tại chi nhánh. Cấu hình Menu, Giá, Định mức (BOM). |

### **5.3. Mô hình Dữ liệu Quan hệ Mở rộng (Enhanced ERD Specification)**

Để hỗ trợ các nghiệp vụ trên, cơ sở dữ liệu (CSDL) cần được chuẩn hóa (Normalization) và mở rộng. Không thể sử dụng cấu trúc đơn giản như Báo cáo Cơ sở.

#### **5.3.1. Bảng Product\_Modifier\_Group (Nhóm Tùy chọn)**

* Mục đích: Quản lý các nhóm tùy chọn (vd: Độ ngọt, Topping, Loại sốt).  
* Các trường:  
  * id (PK)  
  * name (Varchar): Tên nhóm (vd: "Topping Mì").  
  * is\_required (Boolean): Bắt buộc chọn hay không?  
  * min\_selection, max\_selection (Int): Chọn tối thiểu/tối đa (vd: Tối đa 3 loại sốt).

#### **5.3.2. Bảng Product\_Modifier (Chi tiết Tùy chọn)**

* Mục đích: Định nghĩa từng tùy chọn cụ thể.  
* Các trường:  
  * id (PK)  
  * group\_id (FK): Thuộc nhóm nào.  
  * name (Varchar): Tên (vd: "Thêm Trứng Luộc").  
  * price\_adjustment (Decimal): Giá cộng thêm (vd: 5,000 VND).  
  * inventory\_item\_id (FK): Liên kết với nguyên liệu trong kho để trừ tồn.  
  * quantity\_deducted (Float): Lượng trừ (vd: 1 cái).

#### **5.3.3. Bảng Recipe\_Version (Phiên bản Công thức)**

* Mục đích: Quản lý lịch sử thay đổi công thức (rất quan trọng để kiểm soát giá vốn \- COGS).  
* Các trường:  
  * id (PK)  
  * product\_id (FK)  
  * version\_code (Varchar)  
  * effective\_date (Datetime): Ngày áp dụng.  
  * instructions (Text): Hướng dẫn chế biến (hiển thị trên KDS).

#### **5.3.4. Bảng Kitchen\_Routing (Định tuyến Bếp)**

* Mục đích: Cấu hình món nào hiện ở màn hình nào.  
* Các trường:  
  * product\_id (FK)  
  * station\_id (FK): ID của trạm (Bếp nóng, Bar, Salad).

### **5.4. Yêu cầu Chức năng Chi tiết (Functional Requirements)**

#### **Module 1: Quản lý Công thức & Định mức (Recipe Management)**

* **FR-REC-01 (Multi-level BOM):** Hệ thống phải hỗ trợ công thức đa cấp. Ví dụ: Món "Cơm Gà" gồm "Gà Chiên" \+ "Cơm Trắng". "Gà Chiên" lại có công thức riêng từ "Gà Tươi" \+ "Bột". Khi bán Cơm Gà, hệ thống phải trừ lùi (explode) đến nguyên liệu gốc.  
* **FR-REC-02 (Unit Conversion):** Tự động quy đổi đơn vị. Nhập kho theo "Thùng", Công thức dùng "Gram/Mililit".

#### **Module 2: Hệ thống Hiển thị Bếp (KDS)**

* **FR-KDS-01 (Real-time Sync):** Độ trễ từ khi POS chốt đơn đến khi hiện trên KDS phải \< 2 giây.11  
* **FR-KDS-02 (Aging Alert):**  
  * 0-3 phút: Màu Xanh.  
  * 3-5 phút: Màu Vàng (Cảnh báo).  
  * 5 phút: Màu Đỏ (Quá hạn \- Chớp nháy).  
* **FR-KDS-03 (Batch View):** Chế độ xem tổng hợp (Summary View) cho phép bếp biết: "Hiện tại cần tổng cộng 5 phần bò hầm cho 5 đơn khác nhau" để nấu một lần.

#### **Module 3: Quản lý Kho Chế biến (Production Inventory)**

* **FR-INV-01 (Production Log):** Chức năng ghi nhận việc sơ chế. Ví dụ: Nhân viên nhập "Đã luộc 30 trứng". Hệ thống trừ 30 trứng sống, cộng 30 trứng luộc vào kho, và set hạn sử dụng trứng luộc là 24h.  
* **FR-INV-02 (Smart Waste):** Giao diện hủy hàng nhanh cho bếp. Chọn lý do hủy: "Rơi xuống đất", "Quá nhiệt", "Hết hạn". Dữ liệu này dùng để phân tích hiệu quả nhân viên.

## ---

**6\. Tích hợp Quản lý An toàn Thực phẩm & Quy trình Chuẩn (SOP)**

Dựa trên các tài liệu tham khảo về SOP ngành dịch vụ thực phẩm 7, hệ thống phần mềm không chỉ quản lý số liệu mà còn phải là công cụ thực thi quy trình an toàn (Compliance Tool).

### **6.1. Quy trình Kiểm soát Nhiệt độ Số hóa (Digital Logbook)**

* **Vấn đề:** Các cửa hàng thường dùng sổ tay ghi nhiệt độ tủ mát/tủ đông, dễ bị nhân viên ghi khống (fake data).  
* **Giải pháp Hệ thống:**  
  * Tích hợp module "Sổ nhật ký số".  
  * Yêu cầu nhân viên nhập nhiệt độ tủ lưu mẫu 4 tiếng/lần trên ứng dụng mobile.  
  * Tốt hơn: Tích hợp cảm biến IoT, tự động gửi dữ liệu nhiệt độ về hệ thống trung tâm. Nếu nhiệt độ tủ mát \> 5°C (41°F) 8, hệ thống gửi cảnh báo SMS cho Quản lý ngay lập tức.

### **6.2. Kiểm soát Rửa tay và Vệ sinh**

* **Quy định:** Nhân viên phải rửa tay 20 giây bằng xà phòng và nước ấm.7  
* **Thực thi trên phần mềm:**  
  * KDS sẽ bị khóa (Lock screen) sau mỗi 60 phút hoạt động.  
  * Thông báo: "Đến giờ vệ sinh tay & dụng cụ".  
  * Nhân viên phải thực hiện và nhấn "Xác nhận" để mở khóa màn hình làm việc tiếp.

### **6.3. Quản lý Hàng Hủy (Waste Management)**

* **Quy trình:** Hàng tươi sống hết hạn phải được hủy và ghi nhận để hoàn thuế hoặc báo cáo tài chính.13  
* **Hệ thống:**  
  * Sản phẩm "Cơm nắm" có hạn lúc 14:00.  
  * Lúc 13:30, hệ thống gửi thông báo "Cảnh báo sắp hết hạn" xuống KDS/POS để nhân viên dán tem giảm giá (Clearance) nếu có chính sách.  
  * Lúc 14:00, POS tự động chặn mã vạch này. Nhân viên quét mã vào mục "Hủy hàng hết hạn".

## ---

**7\. Kế hoạch Triển khai & Chuyển đổi**

Để đưa hệ thống từ lý thuyết vào thực tế, cần một lộ trình triển khai cụ thể, tránh "sốc" văn hóa cho nhân viên vốn quen làm thủ công.

### **7.1. Giai đoạn 1: Chuẩn hóa Dữ liệu (Data Cleansing)**

* Xây dựng lại toàn bộ danh mục sản phẩm. Tách mã: Mã bán (Sales SKU) và Mã kho (Inventory SKU).  
* Đo lường định mức (BOM) chính xác: 1 tô mì dùng chính xác bao nhiêu gam sốt? (Cần cân đo thực tế trung bình).

### **7.2. Giai đoạn 2: Pilot tại 1 Cửa hàng**

* Lắp đặt màn hình KDS cảm ứng tại khu vực bếp.  
* Chạy song song (Parallel Run): Vừa dùng phiếu giấy vừa dùng KDS trong 2 tuần để nhân viên quen thao tác.

### **7.3. Giai đoạn 3: Đào tạo & Rollout**

* Đào tạo nhân viên bán thời gian về ý nghĩa của việc bấm nút "Hoàn thành" trên KDS (ảnh hưởng đến KPI tốc độ phục vụ).  
* Triển khai toàn chuỗi và kích hoạt tính năng trừ kho tự động.

## ---

**8\. Kết luận**

Báo cáo giữa kỳ 'main 1.pdf' đã đặt những viên gạch đầu tiên cho việc xây dựng hệ thống quản lý cửa hàng tiện lợi, tuy nhiên, nó vẫn mang nặng tính lý thuyết của mô hình bán lẻ truyền thống và chưa chạm tới được độ phức tạp của mô hình "Retail \+ Food Service" hiện đại.

Thông qua việc phân tích sâu sắc các quy trình nghiệp vụ của 7-Eleven và Circle K, báo cáo này đã:

1. **Chỉ ra các lỗ hổng cốt lõi:** Sự tắc nghẽn trong luồng đồng bộ, thiếu quản lý công thức động, và sự vắng mặt của KDS.  
2. **Đề xuất giải pháp toàn diện:** Một kiến trúc hệ thống hướng sự kiện, lấy KDS làm trung tâm vận hành bếp, và tích hợp các quy chuẩn an toàn thực phẩm nghiêm ngặt.

Việc nâng cấp hệ thống theo bản đặc tả này không chỉ giúp hoàn thiện bài tập lớn về mặt học thuật mà còn tạo ra một sản phẩm phần mềm có giá trị thương mại cao, sát sườn với nhu cầu cấp thiết của ngành bán lẻ Việt Nam trong kỷ nguyên số hóa. Đây là bước chuyển mình từ một "phần mềm quản lý kho" thành một "hệ điều hành doanh nghiệp" thực thụ.

---

*Lưu ý: Báo cáo này sử dụng các nguồn dữ liệu từ tài liệu cung cấp và kiến thức chuyên ngành để tổng hợp. Các trích dẫn được đặt tại các điểm thông tin tương ứng để đảm bảo tính xác thực.*

#### **Nguồn trích dẫn**

1. Đề tài thảo luận Nhóm 6 \- MVQTNC \- Scribd, truy cập vào tháng 1 22, 2026, [https://www.scribd.com/document/726528379/%C4%90%E1%BB%81-tai-th%E1%BA%A3o-lu%E1%BA%ADn-Nhom-6-MVQTNC-1](https://www.scribd.com/document/726528379/%C4%90%E1%BB%81-tai-th%E1%BA%A3o-lu%E1%BA%ADn-Nhom-6-MVQTNC-1)  
2. main 1.pdf  
3. 7-Eleven \- Supply Chain World, truy cập vào tháng 1 22, 2026, [https://scw-mag.com/news/733-7-eleven/](https://scw-mag.com/news/733-7-eleven/)  
4. 7-Eleven and Exel Deliver Fresh Food Daily \- Logistics Viewpoints, truy cập vào tháng 1 22, 2026, [https://logisticsviewpoints.com/2014/10/08/7-eleven-and-exel-deliver-fresh-food-daily/](https://logisticsviewpoints.com/2014/10/08/7-eleven-and-exel-deliver-fresh-food-daily/)  
5. Làm Thế Nào để Trả Lời Câu Hỏi Phỏng Vấn Của Circle K Tốt Nhất? \- Tuyendung3s, truy cập vào tháng 1 22, 2026, [https://tuyendung3s.com/cau-hoi-phong-van-circle-K](https://tuyendung3s.com/cau-hoi-phong-van-circle-K)  
6. Review menu Circle K: Có gì khiến giới trẻ muốn "ghé thăm" mỗi ngày? \- iPOS.vn, truy cập vào tháng 1 22, 2026, [https://ipos.vn/review-menu-circle-k/](https://ipos.vn/review-menu-circle-k/)  
7. Standard operating procedures (SOPs) for foodservice | UMN Extension, truy cập vào tháng 1 22, 2026, [https://extension.umn.edu/food-service-industry/standard-operating-procedures-sops](https://extension.umn.edu/food-service-industry/standard-operating-procedures-sops)  
8. \#10- Storing Food Standard Operating Procedure, truy cập vào tháng 1 22, 2026, [https://dpi.wi.gov/sites/default/files/imce/school-nutrition/pdf/storing-food-sop.pdf](https://dpi.wi.gov/sites/default/files/imce/school-nutrition/pdf/storing-food-sop.pdf)  
9. Kitchen display system overview \- \- Platform guide, truy cập vào tháng 1 22, 2026, [https://doc.toasttab.com/doc/platformguide/platformKDSOverview.html](https://doc.toasttab.com/doc/platformguide/platformKDSOverview.html)  
10. Kitchen display system (KDS) | Lightspeed POS, truy cập vào tháng 1 22, 2026, [https://www.lightspeedhq.com/pos/restaurant/kitchen-display-system/](https://www.lightspeedhq.com/pos/restaurant/kitchen-display-system/)  
11. Restaurant's Guide to Kitchen Display Systems (KDS), truy cập vào tháng 1 22, 2026, [https://www.webstaurantstore.com/article/1002/kitchen-display-systems.html](https://www.webstaurantstore.com/article/1002/kitchen-display-systems.html)  
12. Fixed Food Establishment Standard Operating Procedures Manual \- State of Michigan, truy cập vào tháng 1 22, 2026, [https://www.michigan.gov/mdard/-/media/Project/Websites/mdard/documents/food-dairy/pr/fixed\_establishment\_sop\_manual\_form\_fillable.pdf?rev=db66211939bd4016a3b99685d54e0385\&hash=46A0E4372109D46AE7C18725D1BC131A](https://www.michigan.gov/mdard/-/media/Project/Websites/mdard/documents/food-dairy/pr/fixed_establishment_sop_manual_form_fillable.pdf?rev=db66211939bd4016a3b99685d54e0385&hash=46A0E4372109D46AE7C18725D1BC131A)  
13. HƯỚNG DẪN THỰC HIỆN TIÊU HỦY HÀNG HÓA LOẠI HÌNH GC, SXXK, DNCX., truy cập vào tháng 1 22, 2026, [https://damvietxnk.weebly.com/blog/huong-dan-thuc-hien-tieu-huy-hang-hoa-loai-hinh-gc-sxxk-dncx](https://damvietxnk.weebly.com/blog/huong-dan-thuc-hien-tieu-huy-hang-hoa-loai-hinh-gc-sxxk-dncx)