# 本地端 Git 推送到遠端 GitHub

在本地端執行建立 git repository 指令的時候，可以發現 git branch 是 master，但是 GitHub 上面是 main 分支，這時候如果推上 GitHub 的時候，會發現他建立了兩個分支，但我只想要 main 分支就好了，該怎麼解決此問題呢?

原來一切就在 GitHub 上面建立空的儲存庫的時候，他有列出一些指令，只要跟著這些指令就能將 master 分支改為 main 分支了。

下面是 GitHub 上面空的儲存庫提供的指令

```sh
echo "# AboutMe" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/BingFengHung/AboutMe.git
git push -u origin main
```

上面有的 git branch -M main 指令就是關鍵所在，這個指令能夠將當前 git 所在的分支命名，使用 git init 指令之後，我們會再 master 分之下，所以在下 git branch -M main，就能夠將 master 分支重新命名為 main 分支了。

再來後面的指令 git remote add 那一段，由於是第一次要從本地端推送到遠端，當然要讓他知道遠端是誰，所以 origin 就是指遠端的意思。

```
origin: 是一個常用的預設名稱，通常表示主要的遠端分支
```

最後在 git push 指令那邊可以看到有一個 -u，由於是第一次推送，所以要用這個選項將指定的本地端分支設定成追蹤遠端的分支，這樣在執行 git push 的時候 GIT 才會知道是要與哪個遠端分支同步。
