# 概觀

## Line/LIFF

LINE 前端框架\(LIFF\) 是一個讓 Web App 跑在 LINE 裡面的平台

> 這樣的 Web App 我們把它稱為 **LIFF APP**

當你在 LINE 裡面啟動了一個註冊過的 **LIFF APP** 後，這個 **LIFF APP** 就可以：

* 從 LINE 平台取得入 LINE user ID 之類的資料
* **LIFF APP** 可以使用這類使用者資料提供特定功能，也可以以使用者的名義發送訊息。

### 執行環境

* iOS：iOS 8 或更新的版本，iOS 8 跑在 [UIWebView](https://developer.apple.com/documentation/uikit/uiwebview)， iOS 9 之後則是跑在 [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) 裡面
* Android：4.2\(Jelly Bean MR1 及 API level 17\) 或之後的版本
* LINEL：7.14 或之後的版本

### LIFF APP 的畫面大小

![](&&&SFLOCALFILEPATH&&&viewTypes-4cb714f3.png) 在把你的 app 加到 LIFF 時設定畫面大小，目前僅提供 3 種大小，如上圖\( Compact, Tall 及 Full\)

### 開始使用 LIFF

按照下面步驟把你的 Web App 加到 LIFF 裡面。 

1. 在 console 上建立你 **LIFF app** 的頻道\[通道\]
2. 開發 **LIFF app** 
3. 將 Web App 加到 **LIFF**

### 開發準則

確保在開發 Web App 時遵守下列準則：

* 因為以 **LIFF app** 打開頁面的 URL 片段包含 存取 token 及 user ID，所以請小心資料外洩
* 當你實作如取得位置資訊及存取相機和麥克風之類使用裝置或 OS 功能的 API 時，要實作 API 給使用者觸法 API 呼叫。
* 不能在未經使用者同意而以 cookies，localStorage 或外部 session 連結 LINE 使用者資訊追蹤 user 行為。
* 我們不保證可以透過 **LIFF app** 儲存 cookies 或 localStorage。將來可能會受到限制。
* 在 **LIFF app** 測試階段期間，限制 web app 存取 **LIFF app** 的權限。
* **LIFF app** 上及以 **LIFF app** 開啟的任何內容的 URL scheme 必須使用 **https**。如果 URL scheme 時 http，那麼內容就只會在 LINE 的 in-app 瀏覽器中開啟。在這種情況下，即使 Web App 註冊為 **LIFF app** 也無法用 **LIFF app** 的功能。

