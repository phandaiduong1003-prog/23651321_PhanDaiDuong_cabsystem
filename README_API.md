                                               |
# 11. Mô tả API (API Documentation)

> **Lưu ý:** Các endpoint, tên field và cấu trúc JSON dưới đây là **thiết kế kỹ thuật đề xuất** dựa trên Business Requirements, Functional Requirements và Use Case của hệ thống CAB. Tài liệu nghiệp vụ ban đầu chưa quy định cụ thể URL hay schema API.

## 11.1 Thông tin chung

- **API Title:** CAB System API v1.0
- **Base URL đề xuất:** `http://localhost:8080/api/v1`
- **Định dạng dữ liệu:** JSON
- **Content-Type:** `application/json`
- **Xác thực:** Bearer Token đối với các API yêu cầu đăng nhập.

### Header chung

| Header | Bắt buộc | Giá trị | Mô tả |
| --- | --- | --- | --- |
| `Content-Type` | Có với POST/PUT/PATCH | `application/json` | Dữ liệu gửi lên ở dạng JSON. |
| `Authorization` | Có với API cần đăng nhập | `Bearer <access_token>` | Token xác thực người dùng. |

### Cấu trúc lỗi chung

```json
{
  "error_code": "ERROR_CODE",
  "message": "Mô tả nguyên nhân lỗi"
}
```

## 11.2 Danh sách Endpoint

| STT | Nhóm chức năng | Method | Endpoint | Mô tả |
| ---: | --- | --- | --- | --- |
| 1 | Tài khoản | POST | `/auth/register` | Đăng ký tài khoản khách hàng. |
| 2 | Tài khoản | POST | `/auth/login` | Đăng nhập hệ thống. |
| 3 | Tài khoản | GET | `/users/me` | Lấy thông tin hồ sơ của người đang đăng nhập. |
| 4 | Tài khoản | PATCH | `/users/me` | Cập nhật thông tin cá nhân. |
| 5 | Tài xế | PATCH | `/drivers/me/profile` | Cập nhật hồ sơ tài xế và thông tin phương tiện. |
| 6 | Tài xế | PATCH | `/drivers/me/status` | Chuyển trạng thái hoạt động của tài xế. |
| 7 | Đặt xe | POST | `/bookings` | Tạo yêu cầu đặt xe. |
| 8 | Đặt xe | GET | `/bookings/{bookingId}` | Xem thông tin yêu cầu đặt xe. |
| 9 | Điều phối | GET | `/drivers/me/trip-offers` | Lấy lời mời chuyến dành cho tài xế. |
| 10 | Điều phối | POST | `/trip-offers/{offerId}/accept` | Tài xế chấp nhận lời mời chuyến. |
| 11 | Điều phối | POST | `/trip-offers/{offerId}/reject` | Tài xế từ chối lời mời chuyến. |
| 12 | Chuyến đi | GET | `/trips/{tripId}` | Xem thông tin và trạng thái chuyến. |
| 13 | Chuyến đi | PATCH | `/trips/{tripId}/status` | Tài xế cập nhật trạng thái chuyến. |
| 14 | Vị trí | POST | `/drivers/me/location` | Tài xế gửi vị trí hiện tại lên hệ thống. |
| 15 | Theo dõi | GET | `/trips/{tripId}/tracking` | Khách hàng theo dõi trạng thái và vị trí tài xế. |
| 16 | Cước phí | GET | `/trips/{tripId}/fare` | Lấy số tiền cần thanh toán của chuyến. |
| 17 | Thanh toán | POST | `/trips/{tripId}/payments` | Tạo giao dịch thanh toán cho chuyến. |
| 18 | Lịch sử | GET | `/customers/me/trips` | Lấy lịch sử các chuyến của khách hàng. |
| 19 | Đánh giá | POST | `/trips/{tripId}/ratings` | Gửi đánh giá tài xế sau chuyến đi. |
| 20 | Vận hành | GET | `/admin/customers` | Nhân viên vận hành tra cứu khách hàng. |
| 21 | Vận hành | GET | `/admin/drivers` | Nhân viên vận hành tra cứu tài xế. |
| 22 | Vận hành | PATCH | `/admin/drivers/{driverId}` | Cập nhật thông tin tài xế theo quyền vận hành. |
| 23 | Vận hành | GET | `/admin/trips` | Theo dõi danh sách chuyến và trạng thái chuyến. |
| 24 | Vận hành | POST | `/admin/incidents` | Ghi nhận trường hợp chuyến gặp sự cố. |
| 25 | Giao dịch | GET | `/admin/transactions` | Tra cứu lịch sử giao dịch thanh toán. |
| 26 | Báo cáo | GET | `/admin/reports/overview` | Xem báo cáo số chuyến, doanh thu, tỷ lệ hoàn thành và hủy. |
| 27 | Báo cáo | GET | `/admin/reports/drivers` | Xem báo cáo hiệu quả hoạt động của tài xế. |

---

## 11.3 API đăng ký tài khoản

### Endpoint

`POST /api/v1/auth/register`

### Mục đích

Cho phép khách hàng tạo tài khoản mới trên hệ thống.

### Header Parameters

```http
Content-Type: application/json
```

### Message Payload (Request)

```json
{
  "full_name": "Nguyen Van A",
  "phone": "0901234567",
  "email": "nguyenvana@example.com",
  "password": "********"
}
```

### Message Payload (Response)

**201 Created**

```json
{
  "id": "CUS001",
  "full_name": "Nguyen Van A",
  "phone": "0901234567",
  "email": "nguyenvana@example.com",
  "role": "CUSTOMER"
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `201 Created` | Tạo tài khoản thành công. |
| `400 Bad Request` | Dữ liệu gửi lên thiếu hoặc không hợp lệ. |
| `409 Conflict` | Số điện thoại hoặc email đã tồn tại. |
| `500 Internal Server Error` | Lỗi xử lý phía hệ thống. |

### Error Response

```json
{
  "error_code": "ACCOUNT_ALREADY_EXISTS",
  "message": "Số điện thoại hoặc email đã được sử dụng."
}
```

---

## 11.4 API đăng nhập

### Endpoint

`POST /api/v1/auth/login`

### Mục đích

Xác thực người dùng trước khi truy cập các chức năng yêu cầu đăng nhập.

### Message Payload (Request)

```json
{
  "phone": "0901234567",
  "password": "********"
}
```

### Message Payload (Response)

**200 OK**

```json
{
  "access_token": "<token>",
  "token_type": "Bearer",
  "user": {
    "id": "CUS001",
    "full_name": "Nguyen Van A",
    "role": "CUSTOMER"
  }
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Đăng nhập thành công. |
| `400 Bad Request` | Thiếu thông tin đăng nhập. |
| `401 Unauthorized` | Thông tin xác thực không chính xác. |
| `500 Internal Server Error` | Lỗi xử lý phía hệ thống. |

### Error Response

```json
{
  "error_code": "INVALID_CREDENTIALS",
  "message": "Thông tin đăng nhập không chính xác."
}
```

---

## 11.5 API cập nhật trạng thái hoạt động của tài xế

### Endpoint

`PATCH /api/v1/drivers/me/status`

### Mục đích

Cho phép tài xế chuyển đổi trạng thái làm việc để hệ thống xác định khả năng nhận chuyến.

### Header Parameters

```http
Authorization: Bearer <access_token>
Content-Type: application/json
```

### Message Payload (Request)

```json
{
  "status": "ONLINE"
}
```

Giá trị trạng thái sử dụng trong phạm vi hiện tại:

- `ONLINE`: Sẵn sàng nhận chuyến.
- `OFFLINE`: Không nhận chuyến.
- `BUSY`: Đang thực hiện chuyến; trạng thái này do hệ thống cập nhật khi tài xế đã nhận chuyến.

### Message Payload (Response)

```json
{
  "driver_id": "DRV001",
  "status": "ONLINE",
  "message": "Cập nhật trạng thái thành công."
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Cập nhật thành công. |
| `400 Bad Request` | Giá trị trạng thái không hợp lệ. |
| `401 Unauthorized` | Chưa đăng nhập hoặc token không hợp lệ. |
| `403 Forbidden` | Người dùng không có quyền tài xế. |

---

## 11.6 API tạo yêu cầu đặt xe

### Endpoint

`POST /api/v1/bookings`

### Mục đích

Cho phép khách hàng nhập điểm đón, điểm đến và loại xe để tạo yêu cầu đặt chuyến.

### Header Parameters

```http
Authorization: Bearer <access_token>
Content-Type: application/json
```

### Message Payload (Request)

```json
{
  "pickup": {
    "latitude": 10.7769,
    "longitude": 106.7009,
    "address": "Điểm đón của khách hàng"
  },
  "destination": {
    "latitude": 10.7626,
    "longitude": 106.6602,
    "address": "Điểm đến của khách hàng"
  },
  "vehicle_type": "CAR_4_SEATS"
}
```

### Message Payload (Response)

**201 Created**

```json
{
  "booking_id": "BKG001",
  "status": "SEARCHING_DRIVER",
  "pickup": {
    "address": "Điểm đón của khách hàng"
  },
  "destination": {
    "address": "Điểm đến của khách hàng"
  },
  "vehicle_type": "CAR_4_SEATS",
  "message": "Yêu cầu đặt xe đã được ghi nhận."
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `201 Created` | Tạo yêu cầu đặt xe thành công. |
| `400 Bad Request` | Điểm đón, điểm đến hoặc loại xe không hợp lệ. |
| `401 Unauthorized` | Khách hàng chưa đăng nhập. |
| `500 Internal Server Error` | Không thể ghi nhận yêu cầu. |

### Error Response

```json
{
  "error_code": "INVALID_BOOKING_DATA",
  "message": "Thông tin điểm đón, điểm đến hoặc loại xe không hợp lệ."
}
```

---

## 11.7 API lấy lời mời chuyến của tài xế

### Endpoint

`GET /api/v1/drivers/me/trip-offers`

### Mục đích

Trả về các lời mời nhận chuyến mà hệ thống điều phối đến tài xế đang đáp ứng điều kiện nhận chuyến.

### URL Parameters

| Parameter | Kiểu | Bắt buộc | Mô tả |
| --- | --- | --- | --- |
| `status` | string | Không | Lọc lời mời, ví dụ `PENDING`. |

Ví dụ:

```http
GET /api/v1/drivers/me/trip-offers?status=PENDING
```

### Header Parameters

```http
Authorization: Bearer <access_token>
```

### Message Payload (Response)

```json
{
  "data": [
    {
      "offer_id": "OFF001",
      "booking_id": "BKG001",
      "pickup_address": "Điểm đón của khách hàng",
      "destination_address": "Điểm đến của khách hàng",
      "vehicle_type": "CAR_4_SEATS",
      "status": "PENDING"
    }
  ]
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Lấy danh sách lời mời thành công. |
| `401 Unauthorized` | Chưa đăng nhập. |
| `403 Forbidden` | Người dùng không có quyền tài xế. |

---

## 11.8 API chấp nhận lời mời chuyến

### Endpoint

`POST /api/v1/trip-offers/{offerId}/accept`

### Mục đích

Cho phép tài xế chấp nhận lời mời. Khi chấp nhận thành công, hệ thống ghi nhận tài xế được phân công và chuyển tài xế sang trạng thái bận.

### Path Parameters

| Parameter | Kiểu | Bắt buộc | Mô tả |
| --- | --- | --- | --- |
| `offerId` | string | Có | Mã lời mời chuyến. |

### Header Parameters

```http
Authorization: Bearer <access_token>
```

### Message Payload (Response)

```json
{
  "trip_id": "TRIP001",
  "booking_id": "BKG001",
  "driver_id": "DRV001",
  "trip_status": "DRIVER_ASSIGNED",
  "driver_status": "BUSY"
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Nhận chuyến thành công. |
| `401 Unauthorized` | Tài xế chưa đăng nhập. |
| `403 Forbidden` | Tài xế không đủ điều kiện nhận chuyến. |
| `404 Not Found` | Không tìm thấy lời mời. |
| `409 Conflict` | Lời mời đã hết hiệu lực hoặc đã được xử lý. |

### Error Response

```json
{
  "error_code": "TRIP_OFFER_EXPIRED",
  "message": "Lời mời chuyến đã hết hiệu lực."
}
```

---

## 11.9 API từ chối lời mời chuyến

### Endpoint

`POST /api/v1/trip-offers/{offerId}/reject`

### Mục đích

Ghi nhận tài xế từ chối lời mời để hệ thống tiếp tục tìm tài xế phù hợp tiếp theo.

### Path Parameters

| Parameter | Kiểu | Bắt buộc | Mô tả |
| --- | --- | --- | --- |
| `offerId` | string | Có | Mã lời mời chuyến. |

### Message Payload (Response)

```json
{
  "offer_id": "OFF001",
  "status": "REJECTED",
  "message": "Đã ghi nhận từ chối chuyến."
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Ghi nhận từ chối thành công. |
| `404 Not Found` | Không tìm thấy lời mời. |
| `409 Conflict` | Lời mời không còn ở trạng thái chờ xử lý. |

---

## 11.10 API cập nhật trạng thái chuyến đi

### Endpoint

`PATCH /api/v1/trips/{tripId}/status`

### Mục đích

Cho phép tài xế cập nhật các giai đoạn thực hiện chuyến như đã tới điểm đón, đã đón khách, đang di chuyển và hoàn thành.

### Path Parameters

| Parameter | Kiểu | Bắt buộc | Mô tả |
| --- | --- | --- | --- |
| `tripId` | string | Có | Mã chuyến đi. |

### Message Payload (Request)

```json
{
  "status": "ARRIVED_PICKUP"
}
```

Các trạng thái chính:

- `DRIVER_ASSIGNED`: Đã phân công tài xế.
- `ARRIVED_PICKUP`: Tài xế đã tới điểm đón.
- `PASSENGER_PICKED_UP`: Đã đón khách.
- `IN_PROGRESS`: Chuyến đang được thực hiện.
- `COMPLETED`: Chuyến đã hoàn thành.
- `NEEDS_SUPPORT`: Chuyến phát sinh vấn đề cần xử lý.

### Message Payload (Response)

```json
{
  "trip_id": "TRIP001",
  "status": "ARRIVED_PICKUP",
  "message": "Cập nhật trạng thái chuyến thành công."
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Cập nhật trạng thái thành công. |
| `400 Bad Request` | Trạng thái không hợp lệ hoặc sai thứ tự nghiệp vụ. |
| `403 Forbidden` | Tài xế không được phân công cho chuyến này. |
| `404 Not Found` | Không tìm thấy chuyến. |

---

## 11.11 API cập nhật vị trí tài xế

### Endpoint

`POST /api/v1/drivers/me/location`

### Mục đích

Ghi nhận vị trí tài xế trong thời gian chuyến diễn ra để hỗ trợ khách hàng theo dõi chuyến.

### Message Payload (Request)

```json
{
  "trip_id": "TRIP001",
  "latitude": 10.7751,
  "longitude": 106.6983,
  "recorded_at": "2026-09-07T10:20:30Z"
}
```

### Message Payload (Response)

```json
{
  "message": "Vị trí đã được cập nhật."
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Cập nhật vị trí thành công. |
| `400 Bad Request` | Tọa độ không hợp lệ. |
| `403 Forbidden` | Tài xế không thuộc chuyến. |
| `404 Not Found` | Không tìm thấy chuyến. |

---

## 11.12 API theo dõi chuyến đi

### Endpoint

`GET /api/v1/trips/{tripId}/tracking`

### Mục đích

Cho phép khách hàng xem trạng thái chuyến, thông tin tài xế và vị trí gần nhất mà hệ thống đang có.

### Path Parameters

| Parameter | Kiểu | Bắt buộc | Mô tả |
| --- | --- | --- | --- |
| `tripId` | string | Có | Mã chuyến cần theo dõi. |

### Message Payload (Response)

```json
{
  "trip_id": "TRIP001",
  "status": "IN_PROGRESS",
  "driver": {
    "id": "DRV001",
    "full_name": "Tran Van B",
    "vehicle_plate": "51A-12345"
  },
  "latest_location": {
    "latitude": 10.7751,
    "longitude": 106.6983,
    "recorded_at": "2026-09-07T10:20:30Z"
  },
  "estimated_arrival": "2026-09-07T10:45:00Z"
}
```

> `estimated_arrival` chỉ trả về khi hệ thống có đủ dữ liệu để ước tính.

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Lấy thông tin theo dõi thành công. |
| `403 Forbidden` | Người dùng không thuộc chuyến này. |
| `404 Not Found` | Không tìm thấy chuyến. |

---

## 11.13 API lấy cước chuyến đi

### Endpoint

`GET /api/v1/trips/{tripId}/fare`

### Mục đích

Trả về số tiền cần thanh toán sau khi chuyến đã hoàn thành.

### Message Payload (Response)

```json
{
  "trip_id": "TRIP001",
  "amount": 125000,
  "currency": "VND",
  "payment_status": "UNPAID"
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Lấy cước thành công. |
| `400 Bad Request` | Chuyến chưa ở trạng thái có thể chốt cước. |
| `404 Not Found` | Không tìm thấy chuyến. |

---

## 11.14 API thanh toán chuyến đi

### Endpoint

`POST /api/v1/trips/{tripId}/payments`

### Mục đích

Ghi nhận phương thức thanh toán. Đối với thanh toán điện tử, CAB gửi yêu cầu đến nhà cung cấp thanh toán bên ngoài và chỉ lưu mã/token giao dịch cần thiết cho đối soát.

### Message Payload (Request)

Ví dụ thanh toán điện tử:

```json
{
  "method": "ELECTRONIC",
  "payment_token": "<provider_token>"
}
```

Ví dụ thanh toán tiền mặt:

```json
{
  "method": "CASH"
}
```

### Message Payload (Response)

```json
{
  "payment_id": "PAY001",
  "trip_id": "TRIP001",
  "amount": 125000,
  "method": "ELECTRONIC",
  "status": "SUCCESS",
  "provider_transaction_id": "TXN001"
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `201 Created` | Giao dịch được tạo và ghi nhận. |
| `400 Bad Request` | Phương thức hoặc dữ liệu thanh toán không hợp lệ. |
| `402 Payment Required` | Nhà cung cấp từ chối/không hoàn tất thanh toán. |
| `404 Not Found` | Không tìm thấy chuyến. |
| `502 Bad Gateway` | Không nhận được phản hồi hợp lệ từ nhà cung cấp thanh toán. |

### Error Response

```json
{
  "error_code": "PAYMENT_FAILED",
  "message": "Thanh toán điện tử không thành công."
}
```

> CAB không lưu trực tiếp PAN/số thẻ, CVV/CVC, OTP hoặc mật khẩu thanh toán.

---

## 11.15 API xem lịch sử chuyến

### Endpoint

`GET /api/v1/customers/me/trips`

### Mục đích

Cho phép khách hàng xem danh sách các chuyến đã thực hiện và số tiền tương ứng.

### URL Parameters

| Parameter | Kiểu | Bắt buộc | Mô tả |
| --- | --- | --- | --- |
| `status` | string | Không | Lọc theo trạng thái chuyến. |
| `page` | integer | Không | Số trang. |
| `limit` | integer | Không | Số phần tử mỗi trang. |

### Message Payload (Response)

```json
{
  "data": [
    {
      "trip_id": "TRIP001",
      "status": "COMPLETED",
      "pickup_address": "Điểm đón của khách hàng",
      "destination_address": "Điểm đến của khách hàng",
      "amount": 125000
    }
  ],
  "page": 1,
  "limit": 10
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Lấy lịch sử thành công. |
| `401 Unauthorized` | Khách hàng chưa đăng nhập. |

---

## 11.16 API đánh giá tài xế

### Endpoint

`POST /api/v1/trips/{tripId}/ratings`

### Mục đích

Cho phép khách hàng đánh giá tài xế sau khi chuyến hoàn thành.

### Message Payload (Request)

```json
{
  "stars": 5,
  "comment": "Tài xế phục vụ tốt."
}
```

### Message Payload (Response)

**201 Created**

```json
{
  "rating_id": "RAT001",
  "trip_id": "TRIP001",
  "stars": 5,
  "message": "Đánh giá đã được ghi nhận."
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `201 Created` | Gửi đánh giá thành công. |
| `400 Bad Request` | Số sao hoặc nội dung đánh giá không hợp lệ. |
| `403 Forbidden` | Chuyến chưa hoàn thành hoặc người dùng không thuộc chuyến. |
| `409 Conflict` | Chuyến đã được đánh giá hoặc đã hết thời gian cho phép đánh giá. |

### Error Response

```json
{
  "error_code": "RATING_NOT_ALLOWED",
  "message": "Chuyến đi không đủ điều kiện để đánh giá."
}
```

---

## 11.17 API vận hành - tra cứu chuyến

### Endpoint

`GET /api/v1/admin/trips`

### Mục đích

Cho phép nhân viên vận hành theo dõi các chuyến và lọc theo trạng thái để hỗ trợ xử lý bất thường.

### URL Parameters

| Parameter | Kiểu | Bắt buộc | Mô tả |
| --- | --- | --- | --- |
| `status` | string | Không | Lọc theo trạng thái chuyến. |
| `driver_id` | string | Không | Lọc theo tài xế. |
| `customer_id` | string | Không | Lọc theo khách hàng. |
| `page` | integer | Không | Số trang. |

### Header Parameters

```http
Authorization: Bearer <access_token>
```

### Message Payload (Response)

```json
{
  "data": [
    {
      "trip_id": "TRIP001",
      "customer_id": "CUS001",
      "driver_id": "DRV001",
      "status": "IN_PROGRESS"
    }
  ],
  "page": 1
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Tra cứu thành công. |
| `401 Unauthorized` | Chưa đăng nhập. |
| `403 Forbidden` | Không có quyền vận hành. |

---

## 11.18 API tra cứu giao dịch

### Endpoint

`GET /api/v1/admin/transactions`

### Mục đích

Cho phép nhân viên có quyền tìm kiếm và xem lịch sử giao dịch thanh toán.

### URL Parameters

| Parameter | Kiểu | Bắt buộc | Mô tả |
| --- | --- | --- | --- |
| `status` | string | Không | Lọc theo trạng thái giao dịch. |
| `method` | string | Không | Lọc theo phương thức thanh toán. |
| `trip_id` | string | Không | Tra cứu theo mã chuyến. |
| `from` | datetime | Không | Thời điểm bắt đầu. |
| `to` | datetime | Không | Thời điểm kết thúc. |

### Message Payload (Response)

```json
{
  "data": [
    {
      "payment_id": "PAY001",
      "trip_id": "TRIP001",
      "amount": 125000,
      "method": "ELECTRONIC",
      "status": "SUCCESS"
    }
  ]
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Tra cứu giao dịch thành công. |
| `403 Forbidden` | Không đủ quyền xem giao dịch. |

---

## 11.19 API báo cáo tổng quan

### Endpoint

`GET /api/v1/admin/reports/overview`

### Mục đích

Tổng hợp các số liệu phục vụ theo dõi hoạt động kinh doanh và vận hành.

### URL Parameters

| Parameter | Kiểu | Bắt buộc | Mô tả |
| --- | --- | --- | --- |
| `from` | date | Không | Ngày bắt đầu kỳ báo cáo. |
| `to` | date | Không | Ngày kết thúc kỳ báo cáo. |

### Message Payload (Response)

```json
{
  "total_trips": 1200,
  "completed_trips": 1080,
  "cancelled_trips": 120,
  "completion_rate": 0.9,
  "cancellation_rate": 0.1,
  "revenue": 156000000,
  "currency": "VND"
}
```

### Response Code

| HTTP Code | Ý nghĩa |
| --- | --- |
| `200 OK` | Tạo báo cáo thành công. |
| `400 Bad Request` | Khoảng thời gian báo cáo không hợp lệ. |
| `403 Forbidden` | Người dùng không có quyền xem báo cáo. |

---

## 11.20 Quy ước mã lỗi đề xuất

| Error Code | HTTP Code | Ý nghĩa |
| --- | ---: | --- |
| `INVALID_INPUT` | 400 | Dữ liệu đầu vào không hợp lệ. |
| `INVALID_CREDENTIALS` | 401 | Thông tin xác thực không chính xác. |
| `UNAUTHORIZED` | 401 | Chưa xác thực. |
| `FORBIDDEN` | 403 | Không đủ quyền thực hiện chức năng. |
| `RESOURCE_NOT_FOUND` | 404 | Không tìm thấy tài nguyên yêu cầu. |
| `ACCOUNT_ALREADY_EXISTS` | 409 | Tài khoản đã tồn tại. |
| `TRIP_OFFER_EXPIRED` | 409 | Lời mời chuyến đã hết hiệu lực. |
| `INVALID_TRIP_STATUS` | 409 | Trạng thái chuyến không cho phép thao tác hiện tại. |
| `PAYMENT_FAILED` | 402 | Thanh toán không thành công. |
| `PAYMENT_PROVIDER_UNAVAILABLE` | 502 | Dịch vụ thanh toán bên ngoài không khả dụng. |
| `INTERNAL_SERVER_ERROR` | 500 | Lỗi không xác định phía hệ thống. |

## 11.21 Quan hệ giữa API và Functional Requirement

| Nhóm API | Functional Requirement liên quan |
| --- | --- |
| `/auth/*`, `/users/me`, `/drivers/me/*` | FR-01 → FR-06, FR-54 → FR-56 |
| `/bookings` | FR-07 → FR-11 |
| `/drivers/me/trip-offers`, `/trip-offers/*` | FR-12 → FR-18 |
| `/trips/{tripId}/status`, `/drivers/me/location`, `/trips/{tripId}/tracking` | FR-19 → FR-26 |
| `/trips/{tripId}/fare`, `/trips/{tripId}/payments` | FR-27 → FR-32 |
| Cơ chế notification nội bộ | FR-33 → FR-38 |
| `/customers/me/trips`, `/trips/{tripId}/ratings` | FR-39 → FR-41 |
| `/admin/customers`, `/admin/drivers`, `/admin/trips`, `/admin/incidents` | FR-42 → FR-47 |
| `/admin/transactions`, `/admin/reports/*` | FR-48 → FR-53 |
| Authorization, RBAC, Audit Log | FR-54 → FR-57 |

