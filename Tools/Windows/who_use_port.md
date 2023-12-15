開啟資源監視器，使用 cmd 輸入以下指令

```bash
perfmon /res
```

開啟後到網路的標籤下，找到監聽連接阜，從裡面查看指定的 port 是被誰占用

在看他的 PID 號碼，再去工作管理員找到該 pid 號碼終止程式即可

或是使用 cmd 的方式終止程式

```bash
taskkill /f /pid 1234
```
