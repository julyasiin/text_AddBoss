---
## 6. CẤU HÌNH QUÀ TẶNG & TỈ LỆ RƠI ĐỒ KHI BOSS CHẾT
Hàm `rewards(Player pl)` quyết định các phần thưởng khi Boss bị hạ gục:
### 1. Rơi vật phẩm không thể giao dịch (Khóa theo tài khoản):
```java
// Rơi từ 1 đến 15 viên vật phẩm ID 1966 tại vị trí của Boss:
Service.getInstance().dropItemMap(this.zone, 
    Util.khongthegiaodich(zone, 1966, Util.nextInt(1, 15), this.location.x, this.location.y, pl.id));
```
### 2. Rơi trang bị Thiên Sứ / Trang bị Siêu Cấp:
```java
// Rơi đồ thiên sứ cấp 3:
Service.getInstance().dropItemMap(this.zone, 
    Util.setsieucaplv3(zone, itemId, 1, this.location.x, this.location.y, pl.id));
```
### 3. Tự tạo ItemMap và gán danh sách Option tùy biến:
```java
ItemMap itemDrop = new ItemMap(this.zone, ITEM_ID, SO_LUONG, this.location.x, this.location.y, pl.id);
// Option 50: Sức đánh +%
itemDrop.options.add(new ItemOption(50, 30)); // +30% SD
// Option 77: HP +%
itemDrop.options.add(new ItemOption(77, 30)); // +30% HP
// Option 30: Không thể giao dịch
itemDrop.options.add(new ItemOption(30, 1));
Service.getInstance().dropItemMap(this.zone, itemDrop);
```
### 4. Tỉ lệ rơi đồ theo Cấp VIP của người tiêu diệt:
```java
int tiLe = 1;
if (pl.vip == 1) tiLe = 2;
else if (pl.vip == 2) tiLe = 3;
else if (pl.vip == 3) tiLe = 5;
else if (pl.vip >= 6) tiLe = 10;
// Cơ chế xác suất rơi:
if (Util.isTrue(tiLe, 30)) {
    // Rơi đồ hiếm nếu trúng tỉ lệ
}
```
---
*Tài liệu được biên soạn đồng bộ với phiên bản NRO ZIC.*
