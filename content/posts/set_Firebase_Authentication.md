---
date: '2025-01-06T15:49:55+08:00'
draft: false
title: 'Firebase Authentication 教學：讓使用者登入變得輕而易舉 🔐'
---
## 前言 🎤
在應用程式中，使用者驗證是不可缺的一環，而 Firebase Authentication 就像是開發者的魔法工具箱🪄！不僅可以快速實現 Email/密碼登入，還支援第三方登入（Google、Facebook 等），更貼心的是，它還內建了 忘記密碼 功能💡！

今天就帶大家一步步實現這些功能，並在下一篇文章搭配 Naive UI 信息彈窗 提升使用者體驗，讓操作更加直覺、友好！✨（¯▿¯）

## 前置作業 📌
### 1. 準備 Firebase 專案
-  登入 Firebase 官方網站，按下建立專案!
-  輸入你的酷酷名稱建立新專案(ﾉ>ω<)ﾉ
    1. 在「專案設定」中選擇平台建立應用程式
    2. 新增 Firebase SDK
    3. 將 Firebase 產生的 apiKey 複製起來！
        

### 2. 安裝 Firebase 套件
- 在本地專案中安裝 Firebase 套件
    ```bash
    npm install firebase
    ```
        
### 3. 設定 Firebase 環境
在專案中新增一個 `firebase-config.js` 檔案，將 Firebase 配置加入：
```javascript
// firebase-config.js
import { initializeApp } from "firebase/app";

const firebaseConfig = {
  apiKey: "你的 API 金鑰",
  authDomain: "你的專案域名",
  projectId: "你的專案 ID",
  storageBucket: "你的儲存空間",
  messagingSenderId: "你的訊息傳送者 ID",
  appId: "你的 App ID",
};

const app = initializeApp(firebaseConfig);
export default app;
```

## 實際操作 🖥️
### 1. 實現 Email/密碼註冊功能
建立 `registerService.js`，實現註冊功能：
```javascript
// Login/service/registerService.js

import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";

const auth = getAuth();

export const registerUser = async (email, password) => {
  try {
    const userCredential = await createUserWithEmailAndPassword(auth, email, password);
    console.log("使用者註冊成功:", userCredential.user);
  } catch (error) {
    console.error("註冊失敗:", error.message);
  }
};

```
在前端 `LoginPage.vue` 前端呼叫 `registerUser()`
```javascript
// Login/LoginPage.vue

import { registerUser } from "./service/registerService.js";

const email = "test@example.com";
const password = "password123";
registerUser(email, password);
```

### 2. 實現 Email/密碼登入功能
在 `loginService.js` 中增加登入功能：
```javascript
// Login/service/loginService.js

import { signInWithEmailAndPassword } from "firebase/auth";

export const loginUser = async (email, password) => {
  try {
    const userCredential = await signInWithEmailAndPassword(auth, email, password);
    console.log("使用者登入成功:", userCredential.user);
  } catch (error) {
    console.error("登入失敗:", error.message);
  }
};
```
在前端 `LoginPage.vue` 前端呼叫 `loginUser()`
```javascript
// Login/LoginPage.vue

import { loginUser } from "./service/loginService.js";

const email = "test@example.com";
const password = "password123";
loginUser(email, password);
```

### 3. 實現 Google 第三方登入
啟用 Firebase Google 登入後建立 `authService.js`，加入以下程式碼：

```javascript
//  Login/service/authService.js

import { GoogleAuthProvider, signInWithPopup } from "firebase/auth";

export const loginWithGoogle = async () => {
  try {
    const provider = new GoogleAuthProvider();
    const userCredential = await signInWithPopup(auth, provider);
    console.log("Google 登入成功:", userCredential.user);
  } catch (error) {
    console.error("Google 登入失敗:", error.message);
  }
};
```
在前端 `LoginPage.vue` 前端呼叫 `loginWithGoogle()`
```javascript
// Login/LoginPage.vue

import { loginWithGoogle } from "./service/authService.js";

loginWithGoogle();
```
## 4. 實現忘記密碼功能
新建 `resetPasswordService.js` 新增忘記密碼功能：
```javascript
// Login/service/resetPasswordService.js

import { getAuth, sendPasswordResetEmail } from "firebase/auth";

export const resetPassword = async (email) => {
  try {
    await sendPasswordResetEmail(auth, email);
    console.log("重設密碼郵件已寄出");
  } catch (error) {
    console.error("密碼重設失敗:", error.message);
  }
};
```
在前端 `LoginPage.vue` 前端呼叫 `resetPassword()`
```javascript
import { resetPassword } from "./service/resetPasswordService.js";

resetPassword("test@example.com");
```

## 5. 實現登出功能
最後，實現登出功能：
```javascript
import { signOut } from "firebase/auth";

export const logoutUser = async () => {
  try {
    await signOut(auth);
    console.log("使用者已登出");
  } catch (error) {
    console.error("登出失敗:", error.message);
  }
};
```
## 測試與驗證 🔄

完成功能後，可以依以下步驟進行測試：
1. 註冊新使用者：檢查 Firebase 控制台是否新增使用者。
2. 登入測試：使用 Email/密碼或 Google 登入，確認控制台有登入紀錄。
3. 忘記密碼測試：輸入已註冊的 Email，檢查是否收到重設密碼郵件📩。
4. 登出測試：確認會話已結束。

## 結語 🎉

透過 Firebase Authentication，我們不僅可以輕鬆實現多種登入功能，還能提供安全的密碼重設服務，讓使用者體驗大幅提升！🔥 快來試試吧，讓你的專案更加完美！ε٩(๑> ₃ <)۶з