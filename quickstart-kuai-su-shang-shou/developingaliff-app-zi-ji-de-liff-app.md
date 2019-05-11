---
description: 本篇主題解釋將 web app 部署為 LIFF App 所需要的 特定 LIFF 工作。
---

# Developing a LIFF app - 開發自己的 LIFF App

### 設定 LIFF app 的標題

在 web app 中 HTML 的 `title` 標籤指定 LIFF app 名稱，這個名稱會顯示在 LIFF app 的標籤列中

```markup
<title>My LIFF app</title>
```

###  整合 LIFF SDK

LIFF SDK 提供了下列功能。

* 初始化 LIFF app。
* 與 LINE 的介面。 

  在 SDK 中併用 client API，你可以在 LINE in-app 瀏覽器中打開任何 URL 或在外部瀏覽器打開並關閉 LIFF app。

請參考[Calling the LIFF API](https://developers.line.biz/en/docs/liff/developing-liff-apps/#calling-liff-api) 取得更多資訊。

指定 LIFF SDK 的 URL：在 `<script>`中指定

```markup
<script src=“https://d.xdline-scdn.net/liff/1.0/sdk.js></script>
```

### 呼叫 LIFF API

你可以由 LIFF SDK 在 LIFF APP 中進行一下操作。

* [初始化 LIFF APP](developingaliff-app-zi-ji-de-liff-app.md#chu-shi-hua-liff-app)
* [開啟 URL](developingaliff-app-zi-ji-de-liff-app.md#url)
* [取得使用者的存取標記\(access token\)](developingaliff-app-zi-ji-de-liff-app.md#qu-de-shi-yong-zhe-de-cun-qu)
* [取得使用者資訊](developingaliff-app-zi-ji-de-liff-app.md#qu-de-shi-yong-zhe)
* [以使用者身分傳送訊息](developingaliff-app-zi-ji-de-liff-app.md#yi-shi-yong-zhe-shen-fen-song-xi)
* [關閉 LIFF APP](developingaliff-app-zi-ji-de-liff-app.md#liff-app)

#### 初始化 LIFF APP

`liff.init()` 這個方法可以取得足夠用來在 LIFF APP 中呼叫各種 API 的必要資訊。

```javascript
liff.init(
    data => {
        // Now you can call LIFF API
    const userId = data.context.userId;
    },
    err => {
        // LIFF initialization failed
    }
);
```

#### 開啟 URL

`liff.openWindow()`可在 LINE in-app 瀏覽器或外部瀏覽器中開啟指定 URL。 以下範例碼為在外部瀏覽器開啟 `http://example.com`

```javascript
liff.openWindow({
    url:’https://example.com’,
    external:true
});
```

可以在 LIFF API 參考資訊中取得更多有關 [liff.openWindow\(\)](https://developers.line.biz/en/reference/liff#open-window) 的使用方法

#### 取得使用者的存取標記

使用 `liff.getAccessToken()` 取得使用者的存取標記\(access token\)

```javascript
const accessToken = liff.getAccessToken();
```

利用 access token 和 [Social AP](https://developers.line.biz/en/docs/social-api/overview/)I 互動。 首先，讓 token 能夠在 LIFF app 伺服器上可用：藉由在 HTTP POST 請求的 header 上加入 access token，然後送出到以下任一個請求：

* `GET https://api.line.me/oauth2/v2.1/verify`：回傳給這個 access token 的 channel ID。檢查回傳的 chjannel ID 來驗證合法的 LIFF App 回傳 access token。參考 Social API 文件 的 [Verify access token](https://developers.line.biz/en/reference/social-api/#verify-access-token) 取得更完整資訊。
* `GET https://api.line.me/v2/profile`：回傳**使用者ID**，**顯示名稱**，**大頭貼**，和**狀態消息**。參考 Social API 文件的 [Get user profile](https://developers.line.biz/en/reference/social-api/#get-user-profile) 取得更完整資訊。要在 LIFF App 中取得使用者資訊可以呼叫 LIFF SDK 的 `liff.getProfile()`方法。

想要了解更多有關 `liff.getAccessToken()`方法的資訊，請參考 LIFF API 文件中的 [liff.getAccessToken\(\)](https://developers.line.biz/en/reference/liff#get-access-token)

#### 取得使用者資訊

`liff.getProfile()`方法可取得目前使用者的使用者資訊。

```javascript
liff.getProfile()
.then(profile => {
    const name = profile.displayName
})
.catch((err) => {
    console.log(‘error’, err);
});
```

參考 LIFF API 文件中的 [liff.getProfile\(\)](https://developers.line.biz/en/reference/liff#get-profile) 取得更詳細資訊。

#### 以使用者身分傳送訊息

`liff.sendMessages()`方法可用來在 LIFF app 被打開的交談畫面中以使用者身分傳送訊息。 單一請求可以傳送最多五個訊息物件。

從 LIFF app 中，我們可以傳送下列類型的 Messaging API 訊息：

* [文字\(Text\)](https://developers.line.biz/en/docs/messaging-api/message-types/#text-messages)
* [影像\(Image\)](https://developers.line.biz/en/docs/messaging-api/message-types/#image-messages)
* [影片\(Video\)](https://developers.line.biz/en/docs/messaging-api/message-types/#video-messages)
* [聲音\(Audio\)](https://developers.line.biz/en/docs/messaging-api/message-types/#audio-messages)
* [位置\(Location\)](https://developers.line.biz/en/docs/messaging-api/message-types/#location-messages)
* [樣板\(Template\)](https://developers.line.biz/en/docs/messaging-api/message-types/#template-messages)。只能設定 action 為 [URI action](https://developers.line.biz/en/docs/messaging-api/actions/#uri-action)
* [\(Flex Message\)](https://developers.line.biz/en/docs/messaging-api/message-types/#flex-messages)

請參考 Messaging API 文件中的 [Message objects](https://developers.line.biz/en/reference/messaging-api#message-objects) 取得更多有關訊息類型的資訊

當訊息送到包含 LINE 官方帳號機器人的聊天室時，LINE 平台會發送 wehook 事件給機器人的 server 端。當影像，影片和聲音訊息透過 `liff.sendMessages()`方法送出時，回來的 webhook 時間中 `contentProvider.type` 屬性值會是 `external`。更多詳細資訊請參考 Messaging API 文件中的 [Message event](https://developers.line.biz/en/reference/messaging-api/#message-event)。

下面的範例碼將會發送 “Hello, World!” 的使用者訊息到 LIFF app 開啟的聊天視窗。

```javascript
liff.sendMessages([
    {
        type : ‘text’,
        text : ’Hello, World!’
    }
])
.then(() => {
    console.log(‘message sent’);
})
.catch((err) => {
    console.log(‘error’,err);
});
```

更多詳細資訊請參考 LIFF API 的 [liff.sendMessages\(\)](https://developers.line.biz/en/reference/liff#send-messages) 方法。

#### 關閉 LIFF APP

`liff.closeWindow()` 方法可以關閉已開啟的 LIFF app。

```javascript
liff.closeWindow();
```

參考 LIFF API 文件中的 [liff.closeWindow\(\)](https://developers.line.biz/en/reference/liff#close-window) 以取得更多資訊。

#### 下一步

