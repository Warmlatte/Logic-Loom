---
title: "使用 Firebase Admin SDK 實現身份驗證中間件教學 🔐✨"
date: 2025-01-08T23:05:38+08:00
draft: false
description: ""
---
## 🎤 前言

嗨～今天我們來聊聊如何用 Firebase Admin SDK 打造一個能保護後端 API 的身份驗證中間件！🎉
不用擔心，這篇教學絕對超簡單，只要跟著步驟操作，保證你可以順利完成！( •̀ ω •́ )✧

## 🍀 前置作業
1. Node.js：確保安裝了最新版本！
2. Firebase 專案：前往 Firebase 控制台 創建專案，並下載服務帳戶密鑰 JSON 文件～
3. 安裝依賴：先用以下指令安裝需要的 npm 包：
     ```bash
    npm install dotenv firebase-admin
    ```
準備完成了嗎？那我們開始吧！ヾ(≧▽≦*)o


## 🔑 實際操作
### 🌟 步驟一：配置 Firebase Admin SDK
在 `firebase.js` 文件中配置 Firebase Admin SDK，用於初始化身份驗證服務。
```javascript
// src/config/firebase.js

import { initializeApp, cert } from "firebase-admin/app";
import { getAuth } from "firebase-admin/auth";
import * as dotenv from "dotenv";

 // 載入環境變數
dotenv.config();

// 初始化 Firebase Admin SDK
initializeApp({
  credential: cert({
    projectId: process.env.FIREBASE_PROJECT_ID,
    clientEmail: process.env.FIREBASE_CLIENT_EMAIL,
    privateKey: process.env.FIREBASE_PRIVATE_KEY.replace(/\\n/g, "\n"), // 處理多行密鑰整合為一行
  }),
});

export const auth = getAuth();
```

#### 🌟 小提醒：
- 使用 `dotenv` 管理環境變數，可以更安全地保存敏感信息！
- `replace(/\\n/g, "\n")` 是為了解決 JSON 格式中多行私鑰的問題，千萬別忘了這步喔！( ˘▽˘)っ


### 📂 步驟二：自動生成 .env 文件
接下來，我們寫個小工具，實現自動生成 .env 文件，超方便！✨

#### 建立生成腳本
新增 `generate-env.js`，內容如下：
```javascript
import fs from "fs";
import path from "path";

// 指定 Firebase JSON 檔案的路徑
const filePath = path.resolve("./src/config/你的-firebase-key.json");
const keyFile = fs.readFileSync(filePath, "utf8");
const keyData = JSON.parse(keyFile);

// 格式化 private_key
const formattedPrivateKey = keyData.private_key.replace(/\n/g, "\\n");

// 生成 .env 文件內容
const envFilePath = path.resolve("./.env");
const envContent = `FIREBASE_PROJECT_ID=${keyData.project_id}\nFIREBASE_CLIENT_EMAIL=${keyData.client_email}\nFIREBASE_PRIVATE_KEY="${formattedPrivateKey}"\n`;

// 寫入 .env 文件
fs.writeFileSync(envFilePath, envContent, "utf8");
console.log(".env 文件生成完成！🎉");
```

#### 執行腳本
```bash
node generate-env.js
```

這樣就會在專案目錄中生成一個 .env 文件，裡面會包含 Firebase 的必要配置喔！٩(ˊᗜˋ*)و


### 🛡️ 步驟三：建立身份驗證中間件
在 `authMiddleware.js` 中新增以下代碼，來實現身份驗證：

```javascript
// middlewares/authMiddleware.js

import { auth } from "../config/firebase.js";

export const authMiddleware = async (req, res, next) => {
  const authHeader = req.headers.authorization;
  if (!authHeader) {
    return res.status(403).json({
      status: 403,
      message: "沒有提供 Authorization header",
    });
  }

  const idToken = authHeader.split(" ")[1];
  if (!idToken) {
    return res.status(403).json({
      status: 403,
      message: "沒有提供 id token",
    });
  }

  try {
    const decodedToken = await auth.verifyIdToken(idToken);
    req.user = decodedToken; // 保存用戶信息
    next();
  } catch (err) {
    res.status(403).json({
      status: 403,
      message: "無效的憑證",
      error: err.message,
    });
  }
};
```

這樣的中間件會幫你驗證每個請求的 Token，確保只有有效用戶可以訪問受保護的路由～( •̀ ω •́ )✧


### 🚀 步驟四：在 Express 中使用中間件

在主伺服器檔案中（例如 index.js），加上中間件來保護路由：
```javascript
import express from "express";
import { authMiddleware } from "./middlewares/authMiddleware.js";

const app = express();
app.use(express.json());

// 受保護的路由
app.get("/protected", authMiddleware, (req, res) => {
  res.status(200).json({
    status: 200,
    message: "成功訪問受保護的路由！",
    user: req.user, // 用戶信息
  });
});

// 啟動伺服器
app.listen(3000, () => {
  console.log("伺服器已啟動！請訪問 http://localhost:3000 🚀");
});
```

試試看吧！🎉 用 Postman 或其他工具，向 /protected 發送帶有 Firebase Token 的請求，應該會看到回傳的用戶信息！(≧∀≦)ゞ


## 🎉 結語

現在我們已經成功學會如何：
1. 初始化 Firebase Admin SDK
2. 自動生成 .env 文件
3. 實現身份驗證中間件
4. 在 Express 中保護路由

是不是很簡單呢？(｡･∀･)ﾉﾞ 如果你覺得這篇教學對你有幫助，別忘了分享給更多需要的人喔！🚀

一起打造更安全、更強大的後端吧～！ヾ(≧▽≦*)o

