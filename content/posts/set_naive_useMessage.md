---
date: '2025-01-07T15:55:04+08:00'
draft: false
title: '用 Naive UI 的 useMessage 打造更友好的使用者操作體驗 🎉'
---
## 前言 🎤

使用者體驗（User Experience, UX）是應用程式設計中不可或缺的一環，而一個即時、直觀的反饋能夠讓使用者更安心、更愉快地操作✨。在這篇文章中，我們將介紹如何使用 Naive UI 提供的 useMessage，為操作增加提示彈窗，讓你的專案從「好用」變成「超好用」！(ゝ∀･)


## 搭配 useMessage 的優勢 💡
> 傳送門 🌀 [Naive UI Message](https://www.naiveui.com/zh-CN/os-theme/components/message )

1. 即時反饋：讓使用者在操作後即刻獲得成功或失敗的回應。
2. 更友好的提示：以視覺化的彈窗代替冷冰冰的 console.log。
3. 簡單易用：只需一行程式碼，即可實現漂亮的提示彈窗。

## 使用步驟 🖥️

### 1. 安裝 Naive UI
如果你的專案還沒有安裝 Naive UI，請先執行以下指令：
   
```bash
npm install naive-ui
```

### 2. 基本使用 useMessage

Naive UI 提供的 `useMessage` 是一個簡單的訊息提示工具，可以用於成功、失敗或警告等提示。使用前，請務必在 `App.vue` 或其他全域組件中掛載 `NMessageProvider`：

```javascript
```vue
<script setup>
import { NMessageProvider } from 'naive-ui';
</script>

<template>
  <n-message-provider>
    <router-view />
  </n-message-provider>
</template>
```
･*･:≡(　ε:) 以下是基本範例
```javascript
<template>
  <n-button @click="showMessage">點我顯示訊息</n-button>
</template>

<script setup>
import { useMessage } from "naive-ui";

const message = useMessage()

const showMessage = () => {
    message.info('我是一般訊息')
    message.success('我是成功訊息')
    message.error('我是失敗訊息')
    message.warning('我是警告訊息')
}
</script>
```


### 3. 搭配 Firebase 功能提升使用者體驗
以下是如何在使用者驗證中加入彈窗提示的範例：

### 邏輯檔案
#### (1) 註冊功能
```javascript
// Login/service/registerService.js

import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";

const auth = getAuth();

export const registerUser = async (email, password) => {
  try {
    const userCredential = await createUserWithEmailAndPassword(auth, email, password);
    return { success: true, user: userCredential.user };
  } catch (error) {
    return { success: false, message: error.message };
  }
};
```

#### (2) 登入功能
```javascript
// Login/service/loginService.js

import { getAuth, signInWithEmailAndPassword } from "firebase/auth";

const auth = getAuth();

export const loginUser = async (email, password) => {
  try {
    const userCredential = await signInWithEmailAndPassword(auth, email, password);
    return { success: true, user: userCredential.user };
  } catch (error) {
    return { success: false, message: error.message };
  }
};
```
#### (3) Google 登入
```javascript
// Login/service/authService.js

import { getAuth, GoogleAuthProvider, signInWithPopup } from "firebase/auth";

const auth = getAuth();

export const loginWithGoogle = async () => {
  try {
    const provider = new GoogleAuthProvider();
    const userCredential = await signInWithPopup(auth, provider);
    return { success: true, user: userCredential.user };
  } catch (error) {
    return { success: false, message: error.message };
  }
};
```

#### (4) 忘記密碼功能
```javascript
// Login/service/resetPasswordService.js

import { getAuth, sendPasswordResetEmail } from "firebase/auth";

const auth = getAuth();

export const resetPassword = async (email) => {
  try {
    await sendPasswordResetEmail(auth, email);
    return { success: true };
  } catch (error) {
    return { success: false, message: error.message };
  }
};
```
#### (5) 登出功能
```javascript
// Login/service/logoutService.js

import { getAuth, signOut } from "firebase/auth";

const auth = getAuth();

export const logoutUser = async () => {
  try {
    await signOut(auth);
    return { success: true };
  } catch (error) {
    return { success: false, message: error.message };
  }
};
```
### 集中彈窗邏輯在 LoginPage.vue
在 `LoginPage.vue` 中，統一處理所有彈窗邏輯，根據 Service 的回傳結果，使用 useMessage 顯示提示。
```javascript
<template>
  <n-input v-model="email" placeholder="輸入 Email" />
  <n-input v-model="password" type="password" placeholder="輸入密碼" />
  <n-button @click="handleRegister">註冊</n-button>
  <n-button @click="handleLogin">登入</n-button>
  <n-button @click="handleGoogleLogin">Google 登入</n-button>
  <n-button @click="handleResetPassword">忘記密碼</n-button>
  <n-button @click="handleLogout">登出</n-button>
</template>

<script setup>
import { ref } from "vue";
import { useMessage } from "naive-ui";
import { registerUser } from "./service/registerService.js";
import { loginUser } from "./service/loginService.js";
import { loginWithGoogle } from "./service/authService.js";
import { resetPassword } from "./service/resetPasswordService.js";
import { logoutUser } from "./service/logoutService.js";

const email = ref("");
const password = ref("");
const message = useMessage();

const handleRegister = async () => {
  const result = await registerUser(email.value, password.value);
  if (result.success) {
    message.success('註冊成功 🎉');
  } else {
    message.error('註冊失敗 💔');
    console.error(result.message);
  }
};

const handleLogin = async () => {
  const result = await loginUser(email.value, password.value);
  if (result.success) {
    message.success('登入成功 🎉');
  } else {
    message.error('登入失敗 💔');
    console.error(result.message);
  }
};

const handleGoogleLogin = async () => {
  const result = await loginWithGoogle();
  if (result.success) {
    message.success('Google 登入成功 🎉');
  } else {
    message.error('Google 登入失敗 💔');
    console.error(result.message);
  }
};

const handleResetPassword = async () => {
  const result = await resetPassword(email.value);
  if (result.success) {
    message.success('密碼重設郵件已寄出 📩，請檢查你的信箱！');
  } else {
    message.error('密碼重設失敗 💔');
    console.error(result.message);
  }
};

const handleLogout = async () => {
  const result = await logoutUser();
  if (result.success) {
    message.success('登出成功 👋');
  } else {
    message.error('登出失敗 💔');
    console.error(result.message);
  }
};
</script>
```

## 結語 🎉
透過 Naive UI 的 useMessage，你可以輕鬆為使用者操作提供即時反饋，提升整體操作體驗。結合 Firebase Authentication，讓你的專案更友好、更專業！快來試試看吧！🚀