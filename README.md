# HƯỚNG DẪN QUẢN LÝ BOSS (THÊM / XÓA / CẤU HÌNH BOSS & BOSS VIP)
> **Dành cho Server Dragon Boy / NRO ZIC**  
> Bản hướng dẫn chi tiết, chuẩn hóa toàn diện cấu trúc file, ID kỹ năng, quy tắc VIP và rơi đồ.
---
## MỤC LỤC
1. [Kiến Trúc & Các File Liên Quan](#1-kiến-trúc--các-file-liên-quan)
2. [Bảng Tra Cứu Toàn Bộ ID Skill Của Boss & Cú Pháp `skillTemp`](#2-bảng-tra-cứu-toàn-bộ-id-skill-của-boss--cú-pháp-skilltemp)
3. [Hướng Dẫn Từng Bước Thêm Một Boss Mới (Step-by-Step)](#3-hướng-dẫn-từng-bước-thêm-một-boss-mới-step-by-step)
4. [Hướng Dẫn Tắt / Xóa Boss Khỏi Server](#4-hướng-dẫn-tắt--xóa-boss-khỏi-server)
5. [Cơ Chế & Điều Kiện Đánh Boss VIP (Chuẩn VIP Bản Thân)](#5-cơ-chế--điều-kiện-đánh-boss-vip-chuẩn-vip-bản-thân)
6. [Cấu Hình Quà Tặng & Tỉ Lệ Rơi Đồ Khi Boss Chết](#6-cấu-hình-quà-tặng--tỉ-lệ-rơi-đồ-khi-boss-chết)
---
## 1. KIẾN TRÚC & CÁC FILE LIÊN QUAN
Hệ thống Boss trong server được chia tách logic rất khoa học qua các file sau:
| File | Đường dẫn | Chức năng chính |
| :--- | :--- | :--- |
| **`BossFactory.java`** | `src/.../nro/models/boss/BossFactory.java` | Khai báo hằng số ID Boss (`bosstheovip1`,...), đăng ký hàm tạo `createBoss(id)`, hàm gọi xuất hiện ban đầu `initBoss()`. |
| **`BossData.java`** | `src/.../nro/models/boss/BossData.java` | Cấu hình chỉ số kỹ thuật: Tên Boss, HP, Dame, Trang phục (Head, Body, Leg), Map xuất hiện, Bộ kỹ năng (`skillTemp`), Thời gian hồi sinh (`secondsRest`). |
| **Class Boss Riêng** | `src/.../nro/models/boss/list_boss/...` | Lớp logic riêng của từng con Boss (kế thừa `FutureBoss` hoặc `Boss`). Xử lý nhận sát thương (`injured`), điều kiện ai được đánh, hiệu ứng, lời thoại (`initTalk`), rơi đồ khi chết (`rewards`). |
| **`Boss.java`** | `src/.../nro/models/boss/Boss.java` | Lớp cha xử lý vòng đời: Sinh ra -> Xuất hiện -> Tấn công -> Chết -> Tặng điểm săn boss (`getPointSb()`). |
---
## 2. BẢNG TRA CỨU TOÀN BỘ ID SKILL CỦA BOSS & CÚ PHÁP `skillTemp`
Trong `BossData.java`, kỹ năng của Boss được khai báo qua mảng 2 chiều `skillTemp`:
```java
.skillTemp(new int[][]{
    {ID_SKILL, LEVEL_CHIEU, DELAY_HOI_CHIEU_MS},
