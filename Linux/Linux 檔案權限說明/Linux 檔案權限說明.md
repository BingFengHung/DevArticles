# Linux 檔案權限說明
使用 Linux 檔案權限查看資訊時，可以看到有 10 個符號

第一個符號如果是 d 代表資料夾、如果是 - 代表一般的檔案

檔案

```bash
-rw-------
```

資料夾

```bash
drwx------
```

接下來，後面九個符號代表的就是檔案的權限了

以三個符號為一組，可分為
- 檔案擁有者的權限
- 相同群組 (group) 的使用者權限
- 其他使用者的權限

各權限代表的意思
- r： 代表允許讀取的權限
- w： 代表允許寫入的權限 
- x： 代表執行的權限

如果要更換檔案的群組可以使用 `chgrp` 指令

如果要更換檔案的擁有者可以使用 `chown` 指令
