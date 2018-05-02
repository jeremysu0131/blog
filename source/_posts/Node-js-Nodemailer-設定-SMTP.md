---
title: '[Node.js] Nodemailer 設定 SMTP'
date: 2018-05-02 17:17:16
categories:
tags:
- Node.js 
- Javascript
- SMTP
---

# 設定

由於目前網路上 Nodemailer 的設定都是舊的寫法，新版本已經無法使用，這裡來做一下 Gmail 與其他 SMTP 的設定。

<!-- more -->

## Gmail 程式碼範例

```js
var nodemailer = require('nodemailer');

var transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: 'youremail@gmail.com',
    pass: 'yourpassword',
  },
});

var mailOptions = {
  from: 'youremail@gmail.com',
  to: 'myfriend@yahoo.com',
  subject: 'Sending Email using Node.js',
  text: 'That was easy!',
};

transporter.sendMail(mailOptions, function(error, info) {
  if (error) {
    console.log(error);
  } else {
    console.log('Email sent: ' + info.response);
  }
});
```

這邊的程式碼是參考 W3C 的[範例](https://www.w3schools.com/nodejs/nodejs_email.asp)，經過測試是可以使用的，但是現在 Gmail 有多了一些設定，所以我們要到 Gmail 內去做一些修改。

### Gmail 設定 IMAP

1.  在電腦上開啟 Gmail。
2.  點選右上角的「設定」圖示 設定。
3.  按一下 [設定]。
4.  點選 [轉寄和 POP/IMAP] 分頁。
5.  在「IMAP 存取」部分中，選取 [啟用 IMAP]。
6.  按一下 [儲存變更]。

### Gmail 設定 APP passwords

接下來，因為把自己的真實賬號密碼放在程式中也會有安全性的問題，所以我們到 Google 去設定一組專門用來寄信的 Key。

1.  在 Google 首頁右上角點選 `我的賬戶`
    ![Step 1](app_password1.png)
2.  選擇 `登入和安全性`
    ![Step 1](app_password1.png)
3.  進入設定頁面後，往下找到 `App passwords` 點選進入
    ![Step 1](app_password1.png)
4.  接下來就可以進入設定頁面，選擇要產生的名稱後按下 `Generate` ，就會得到一組 key

![Step 1](app_password1.png)

5.  接下來我們只要把上面程式碼填入我們剛剛得到的 key，就可以正常使用 SMTP 寄信的功能了

```js
var transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: 'youremail@gmail.com',
    pass: '取得的 key',
  },
});
```

## 其他 SMTP 設定

在公司工作時，基本上會有自己專屬的 email ，所以我們也可以用這組信箱來做設定，但是相關的 資訊可能要跟公司的網管詢問，這邊列出基本要填入的訊息。

```js
var transporter = nodemailer.createTransport({
  host: 'mail.domain.com', // Server
  port: 25,
  auth: {
    user: 'yourcompany@email',
    pass: 'password',
  },
});
```

## 參考資料

1.  [W3C](https://www.w3schools.com/nodejs/nodejs_email.asp)
2.  [Nodemailer](https://nodemailer.com/smtp/)
