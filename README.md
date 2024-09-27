# 用Bash開發一鍵安裝的腳本

## 撰寫動機
腳本（Scripting）是利用腳本語言（如Bash、Python、PowerShell等）來自動化一系列任務的過程，本篇希望能給沒接觸過腳本的學生或菜鳥工程師一個參考，如果有什麼疑問或建議歡迎指教~~  
開發腳本不難，但是沒碰過的工程師其實滿多的，是職場菜鳥可以學一下的加分技能!

許多公司開發專案時，需要安裝許多不同的檔案來建構整個系統，造成整個過程非常繁瑣。當開發完成要交貨給客戶時，也都還要再寫一篇複雜的安裝手冊給客戶看，大部分客戶可能也都看得霧煞煞。
因此撰寫腳本是解決這個問題的方式，在大學去竹科實習期間，剛好有機會學如何撰寫腳本，協助公司簡化安裝過程，讓客戶能一鍵安裝。

因為實習的時候安裝的是Ubuntu 18.04 Server版，所以本篇會教各位怎麼安裝Server版，但如果只想學習腳本開發建議直接裝Desktop版本然後閱讀第二章就好。

## 點擊觀看內容
<details>
<summary>一. 安裝 Ubuntu 18.04 Server</summary>

**安裝步驟：**
1. 前往 [Ubuntu 下載 ISO 檔](https://releases.ubuntu.com/18.04/)
      ![安裝步驟](readme%20image/圖片2.png)
   
2. 使用 [Rufus](https://rufus.ie/zh_TW/) 製作 Ubuntu 開機碟，可以參考 [PYDOING 大大的教學影片](https://www.youtube.com/watch?v=i7Uee78td-s)，下圖是製作完成後開機碟的樣子。  
   開機碟（或Live USB）是指一個可啟動ubuntu的USB隨身碟，一般Desktop版燒錄完打開會是Ubuntu試用版的桌面，可以拿來安裝正式的Ubuntu或修復系統等等。
   
   Server版打開則是直接進入安裝介面，如下圖所示。  
   ![開機碟完成](readme%20image/圖片3.png)  
   ![Desktop版與Server版](readme%20image/圖片4.png)

3. 燒錄完成後變可重新開機，開機時狂按各家廠牌設定的BIOS鍵進入BIOS。  
   (*每台電腦進入bios的按鍵不同，微星是DEL鍵)  
   BIOS是電腦開機第一個被載入的軟體，負責初始化硬體，我們可以在此選擇要用哪個裝置開機。  
   ![補一張啥時進bios](readme%20image/圖片6.png)

4. 選擇使用開機碟裝置來開機，開機後就會進入到安裝介面，這裡基本上就是照著建議裝就好了，只有分割磁碟那裡比較危險要小心不要刪到自己的資料。
   ![補一張磁碟分割](readme%20image/圖片5.jpg)
</details>

<details>
<summary>二. 腳本撰寫過程</summary>

**腳本撰寫：**
腳本其實就只是一堆Bash指令的集合，重點是要如何寫的快速精簡、具可讀性、錯誤處理。那麼這三點是什麼意思又該如何執行呢?  

**快速精簡：**
編寫指令不難，但是要如何寫得有**效率**呢? 筆者認為第一步是要先**設計並熟悉你的資料夾結構**。假設今天您設計了一套軟體，並且要設計如何讓客戶用簡單的方式安裝，可能的結構範例如下:
![補一張資料夾結構](readme%20image/圖片7.png)  
假設您今天設計了一套軟體，要幫客戶的新電腦安裝您的產品，那你的安裝包就可以設計成這樣，包含Ubuntu GUI安裝資料、顯卡驅動(GPU Driver)、您設計的軟體(System Data)、軟體的相關依賴(System Dependencies)、跟其他幫助系統加速的資料等等，每個資料夾的功能定義都簡潔明瞭，這樣不僅能減少撰寫時檢查路徑的時間，客戶端也方便閱讀。  

再來如何**精簡**呢? 假設有一個腳本，它需要多次進行文件壓縮操作，可能像這樣：
```bash
# 壓縮檔案的代碼重複多次
tar -czvf backup1.tar.gz /path/to/dir1
echo "Backup for dir1 completed."

tar -czvf backup2.tar.gz /path/to/dir2
echo "Backup for dir2 completed."

tar -czvf backup3.tar.gz /path/to/dir3
echo "Backup for dir3 completed."
```

那你就可以將重複的部分改為**函數調用**:
```
#!/bin/bash

# 定義壓縮和打印的函數
compress_and_notify() {
    local filename=$1
    local dir=$2

    tar -czvf "$filename" "$dir"
    echo "Backup for $dir completed."
}

# 調用函數來壓縮不同的目錄
compress_and_notify "backup1.tar.gz" "/path/to/dir1"
compress_and_notify "backup2.tar.gz" "/path/to/dir2"
compress_and_notify "backup3.tar.gz" "/path/to/dir3"
```

再來就是**避免冗長的程式與命名**，腳本的目的應該是簡單的任務自動化，保持每個腳本專注於一個功能，不要讓腳本變得過於複雜。
如果功能變得複雜，也可以考慮將其**拆成多個腳本**。
</details>

<details>
<summary>三. 範例應用</summary>
（此處可以繼續描述腳本撰寫的內容）
</details>


