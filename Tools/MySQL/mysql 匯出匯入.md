匯出資料表
mysqldump.exe -u root -p dd --ignore-table=dd.slog > D:\dd.sql ---->  忽略 slog
mysqldump.exe -u root -p dd -d slog > D:\dd.sql  ----> 僅匯出 sp_log 結構

匯入資料表
mysql.exe -u root -p ds < D:\ds.sql