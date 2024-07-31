# MySQL 設定事件自動清理超過指定時間的資料

## 前言
資料庫的儲存空間並不是無窮無盡的，如果說資料表上的紀錄過大時，會導致伺服器崩潰，為了避免資料庫中的資料占用過大，可以寫一支程式定期的去做清理資料庫中比較舊的資料。不過其實 MySQL 裡面就能夠設定定期觸發某個事件的機制，為此，本篇將說明如何自動清理資料庫中，超過指定時間的資料。

## 事件排程器
MySQL 的事件排程器，可以在指定的時間內，執行指定的指令。


### 查詢事件排程器的狀態

```bash
SHOW VARIABLES LIKE 'event_scheduler';
```

### 顯示目前伺服器設定的事件

```bash
SHOW EVENTS;
```

### 刪除伺服器上面指定的事件

```bash
DROP EVENT xxx;
```

### 建立事件

```bash
CREATE EVENT delete_records
ON SCHEDULE EVERY 1 DAY
STARTS (TIMESTAMP(CURRENT_DATE, '13:30:00'))
DO
DELETE FROM table WHERE 
```

上面的語法相當的簡單，可以根據自身需求進行調整。