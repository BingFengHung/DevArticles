# Visual Studio 重試次數超過 10 次。作業失敗。

在使用 Visual Studio 進行編譯時，有時會跑出 「Visual Studio 重試次數超過
10 次。作業失敗。」，有時候，將 Visual Studio
關閉然後重開會有作用，但是有時候又會失敗，導致程式無法順利編譯完成。為此，本篇主要提供說明如何進行排除，讓有相同狀況的使用者可以依循此方向進行問題的排除。

通常，跳出此錯誤訊息時，下面會有夾帶以下訊息
> 使用 Visual Studio 2019 建置專案時出現：
> 
> 無法將 “obj\x86\Debug\xxx.exe” 複製到 “bin\Debug\xxx.exe”。重複次數超過 10 次。作業失敗。
> 
> 無法將檔案”obj\x86\Debug\xxx.exe” 複製到 “bin\Debug\xxx.exe”。由於另一個程序正在使用檔案 “bin\Debug\xxx.exe” ，所以無法存取該檔案。

**解決辦法：**

先到 obj 的資料夾將 xxx.exe 檔案複製到 bin
底下的檔案取代，發現無法取代後，開啟工作管理員檢查是否有執行檔還是開啟的狀態，確認檔案確實被咬住，結束程式後，就可順利執行了。

會有上述的狀況發生，絕大部分都是，某個程式將待編譯的程式咬住，導致無法進行編譯，因此，後續只要碰到相同的問題時，首先可以往是否有其它支程式，或是本身的程式並未正常關閉，這樣就不用每次發生同樣的問題時，還需要重新開啟
Visual Studio，然後還會有失敗的問題。
