# 23651321_PhanDaiDuong_cabsystem

# CAB System – Phân tích nghiệp vụ

## 1. Hệ thống hiện tại có những vấn đề gì?

* Phân công tài xế chủ yếu thực hiện thủ công.
* Khách hàng khó theo dõi trạng thái chuyến đi.
* Thông tin thanh toán chưa được quản lý tập trung.
* Bộ phận vận hành khó mở rộng hệ thống khi số lượng khách hàng và tài xế tăng.

## 2. Mục tiêu của hệ thống

Xây dựng một nền tảng CAB mới nhằm:

* Hỗ trợ số lượng lớn khách hàng và tài xế.
* Tự động hỗ trợ tìm kiếm và phân công tài xế.
* Cho phép khách hàng theo dõi chuyến đi.
* Quản lý thanh toán và thông tin giao dịch tập trung.
* Có khả năng mở rộng và bổ sung chức năng trong tương lai.

## 3. Người tham gia sử dụng hệ thống

| Người tham gia     | Vai trò                                              |
| ------------------ | ---------------------------------------------------- |
| Khách hàng         | Đặt xe, theo dõi chuyến, thanh toán, đánh giá tài xế |
| Tài xế             | Nhận chuyến, thực hiện chuyến, cập nhật trạng thái   |
| Nhân viên vận hành | Quản lý khách hàng, tài xế, phương tiện và chuyến đi |

## 4. Vấn đề của hệ thống

| Vấn đề                    | Mô tả                                                   |
| ------------------------- | ------------------------------------------------------- |
| Phân công tài xế thủ công | Mất thời gian, khó xử lý khi có nhiều yêu cầu           |
| Khó theo dõi chuyến       | Khách hàng không nắm rõ tài xế và trạng thái chuyến     |
| Thanh toán phân tán       | Khó quản lý tập trung thông tin thanh toán              |
| Khó mở rộng               | Hệ thống hiện tại khó đáp ứng khi lượng người dùng tăng |

## 5. Các bên liên quan (Stakeholder)

| Tên                     | Vai trò                                                               |
| ----------------------- | --------------------------------------------------------------------- |
| Khách hàng              | Đặt xe, theo dõi chuyến đi, thanh toán và đánh giá tài xế             |
| Tài xế                  | Nhận chuyến, thực hiện chuyến và cập nhật trạng thái chuyến           |
| Nhân viên vận hành      | Quản lý khách hàng, tài xế, phương tiện và chuyến đi; xử lý các sự cố |
| Ban lãnh đạo            | Theo dõi báo cáo về số lượng chuyến, doanh thu và hiệu quả hoạt động  |
| Nhà cung cấp thanh toán | Xử lý các giao dịch thanh toán điện tử                                |
| Nhà cung cấp thông báo  | Cung cấp các kênh gửi thông báo cho khách hàng và tài xế              |

## Ma trận các bên liên quan

| Bên liên quan | Mức độ ảnh hưởng | Mức độ quan tâm | Chiến lược |
|---|---|---|---|
| **Ban lãnh đạo** | Cao | Cao | Quản lý chặt chẽ |
| **Nhân viên vận hành** | Cao | Cao | Quản lý chặt chẽ |
| **Khách hàng** | Cao | Cao | Quản lý chặt chẽ |
| **Tài xế** | Cao | Cao | Quản lý chặt chẽ |
| **Nhà cung cấp thanh toán** | Cao | Trung bình | Giữ hài lòng |
| **Nhà cung cấp thông báo** | Trung bình | Trung bình | Theo dõi và phối hợp |

### Phân loại Stakeholder

```mermaid
quadrantChart
    title Ma trận các bên liên quan
    x-axis "Mức độ quan tâm thấp" --> "Mức độ quan tâm cao"
    y-axis "Mức độ ảnh hưởng thấp" --> "Mức độ ảnh hưởng cao"
    quadrant-1 "Quản lý chặt chẽ"
    quadrant-2 "Giữ hài lòng"
    quadrant-3 "Theo dõi"
    quadrant-4 "Theo dõi và cung cấp thông tin"

    "Ban lãnh đạo": [0.90, 0.95]
    "Nhân viên vận hành": [0.90, 0.90]
    "Khách hàng": [0.95, 0.80]
    "Tài xế": [0.90, 0.80]
    "Nhà cung cấp thanh toán": [0.55, 0.85]
    "Nhà cung cấp thông báo": [0.50, 0.55]
```
B3 Mục đích của nghiệp vụ

Mục đích của nghiệp vụ trong hệ thống CAB là:

Quản lý quy trình đặt xe từ khi khách hàng tạo yêu cầu đến khi hoàn thành chuyến.
Tìm kiếm và phân công tài xế phù hợp, ưu tiên tài xế gần khách hàng.
Theo dõi trạng thái chuyến đi và cung cấp thông tin cho khách hàng, tài xế.
Tính cước và quản lý thanh toán cho các chuyến xe.
Quản lý thông tin khách hàng, tài xế, phương tiện và chuyến đi tập trung.
Hỗ trợ nhân viên vận hành theo dõi, xử lý các trường hợp phát sinh.
Cung cấp dữ liệu và báo cáo để ban lãnh đạo theo dõi hoạt động kinh doanh.
Xây dựng hệ thống ổn định, bảo mật và có khả năng mở rộng trong tương lai

B4 Xác định phạm vi cần làm cho dự án trong 7 tuần
