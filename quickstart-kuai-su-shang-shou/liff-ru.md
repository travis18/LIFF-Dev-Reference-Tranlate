# LIFF 入門

### Line/LIFF

必須先在 [LINE Developers console](https://developers.line.biz/console/%20) 上建立 channel \[通道\]後才能開始使用 LIFF。

### Channel 是什麼？

**channel** 是一個給供應商開發的服務中使用 LINE 平台提供函式的溝通路徑。要使用 LINE 平台，必須建立一個和你的服務結合的**channel**。**Channel**必須要設定_名稱_，_說明_以及圖示照片。當建立一個新的**channel**時，系統會建立一個唯一的 **channel ID**用來辨識這個**channel**

我們可以在 LIFF App 使用 LINE Login **channel** 以及 Messaging API **channel**

當我們在 LINE Login **channel** 上使用 LIFF server API 時，我們需要發行一個如 [6.Prepare a channel access token](https://developers.line.biz/en/docs/liff/getting-started/#preparing-channel-access-token) 所描述的 存取標記\[access token\]。

**注意：** LIFF 不支援非 LINE Login 以及 Messaging API 以外的 **channel**

### 建立 channel

1. 登入 console

以 email + 密碼或 QR code 登入（必須要有 LINE 帳號） ![](&&&SFLOCALFILEPATH&&&login-dialog-b0bc3091.png) 2. 註冊為開發者（只在第一次登入時） 在 LINE 開發者 console 上輸入 email 以建立開發者帳號 ![](&&&SFLOCALFILEPATH&&&developer-registration-58bb0070.png) 3. 建立新的 App 服務供應商 輸入供應商名稱。所謂供應商即是你提供 App 服務的實體。例如：我們可以用公司名稱作為供應商名稱。 ![](&&&SFLOCALFILEPATH&&&create-provider-9e37f3a8.png) 4. 建立 **channel** 為你的 **channel** 輸入必要資訊。注意“LINE” 或類似文字不能包含在名稱中 ![](&&&SFLOCALFILEPATH&&&create-channel-72cb7577.png) 5. 確認 確認你的**channel**已經成功建立 ![](&&&SFLOCALFILEPATH&&&console-home-b465d14d.png) 6. 準備 **channel** 存取標記\[access token\]

* **在 Messaging API channel** 當 **channel** 建立的同時會發行一個 **channel** 存取標記\[channel access token\]。可以在 “Channel settings” 頁面 “Messaging settings” 區塊中的 “Channel access token \(long-lived\) 欄位查看他的數值。這個標記的有效期很長，無須擔心它會過期。
* **在 LINE Login channel** 呼叫 API 發行存取標記\[access token\]。做法是：送出一個 _HTTP POST_ 請求到 `/v2/oauth/accessToken` 端點\[endpoint\]。API 發行 **channel** 存取標記\[channel access token\] 的有效期限時 30 天。

_請求範例：_

```text
curl -v -X POST https://api.line.me/v2/oauth/accessToken \
-H "Content-Type:application/x-www-form-urlencoded" \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'client_id={channel ID}' \
--data-urlencode 'client_secret={channel secret}'
```

**channel** 存取標記\[access token\]會在成功後回傳。

_回應範例_

```javascript
{
  "access_token":"{channel access token}",
  "expires_in":2592000,
  "token_type":"Bearer"
}
```

參考 Messaging API 參考文件的 [Issue channel access token](https://developers.line.biz/en/reference/messaging-api/#issue-channel-access-token) 了解更多資訊。

### 下一步

我們現在已經為你的 LIFF app 建立了 **channel**。下一步，可以嘗試部署範例 App 或開發自己的 LIFF app。

* [嘗試部署範例 App](https://developers.line.biz/en/docs/liff/trying-liff-app/)
* \[\[開發自己的 LIFF App\]\]

