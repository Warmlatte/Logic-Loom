---
date: '2025-01-05T19:16:11+08:00'
draft: false
title: '為 Express 專案打造互動式 Swagger API 文檔 🌐'
---
## 前言 🎤
在做我的第一個專案時，我們最初用 Notion 當 API 文件，剛開始寫 API 時還會乖乖更新文檔📄，但隨著 API 越寫越多，更新文檔的事情就逐漸被拋諸腦後😅。

而且在前後端分離的情況下，負責前端的組員常常對測試 API 一頭霧水🤔。這時，我靈光一閃，想起老師課堂上提到的超好用圖形化工具✨——Swagger！於是，我開始鑽研如何把 Swagger 引入專案，讓 API 文件和測試一舉搞定🎯！


## 📌 前置作業 

需要用到兩個套件 `swagger-ui-express` & `swagger-autogen`
### 1. swagger-ui-express 
>傳送門 🌀 [swagger-ui-express](https://www.npmjs.com/package/swagger-ui-express )


**什麼是 swagger-ui-express？🖥️**

swagger-ui-express 是一個方便易用的 Express 中介軟體，讓你輕鬆整合 Swagger UI 到你的 Express 應用程式中！✨ 它可以將你的 Swagger 文件以一個互動式的圖形介面展示出來，供開發者進行查閱和測試，徹底解決 API 文件難管理、難測試的痛點💡。

**為什麼選它？**
- 快速整合：幾行程式碼就能把 Swagger UI 加入專案🔧。
- 互動式測試：支援直接從瀏覽器發送請求測試 API 功能🚀。
- 開發者友好：讓團隊中的每個人（不只後端開發者！）都能方便理解和使用 API📖。



### 2. swagger-autogen 
>傳送門 🌀 [swagger-autogen](https://www.npmjs.com/package/swagger-autogen )
>官方文檔 📄 [Quick Start](https://swagger-autogen.github.io/docs/getting-started/quick-start/)


**什麼是 swagger-autogen？✨**

swagger-autogen 是一個專為開發者設計的工具包，可以自動生成符合 Swagger 規範的 API 文件📄！只需撰寫簡單的註解，它就能快速掃描你的程式碼，幫助你生成完整的 Swagger 文件，還能直接與 Swagger UI 整合，讓 API 文件不僅即時更新，還可以用於互動式測試🚀。

**為什麼選擇它？**
- 省時省力：免手動撰寫 JSON 格式的 API 文件。
- 高度自動化：從程式碼註解中自動提取路由資訊📌。
- 即時測試：結合 Swagger UI，讓 API 測試變得超方便🔧。

## 🖥️ 實際操作 
### 1. 安裝套件至你的 express 專案
```bash
npm install swagger-ui-express
npm install swagger-autogen
```
    
### 2. 設定 Swagger 自動生成檔案
在專案根目錄中，建立一個 `swagger.js` 檔案，設定 swagger-autogen 來生成 Swagger 文件。

```javascript
import swaggerAutogen from "swagger-autogen";

const doc = {
  info: {
    title: "我的 API 寶庫",
    description: "我把所有的 API 都放在 localhost:3030 了，去尋找吧！",
  },
  host: "localhost:3030",
};

const outputFile = "./swagger-output.json";
const routes = ["./app.js"];

swaggerAutogen()(outputFile, routes, doc);

```

`require` 的寫法只要把 `import` 換掉就好，剩下相同
```javascript
 const swaggerAutogen = require('swagger-autogen')();
```

執行它，一鍵生成 swagger-output.json！✨
```bash
node swagger.js
```

### 3. 整合 Swagger UI 到 Express 專案
在 app.js 中加入以下程式碼：
```javascript
import express from "express";
const app = express();

// swagger
import swaggerUi from "swagger-ui-express";
import fs from "fs";

const swaggerDocument = JSON.parse(
  fs.readFileSync(new URL("./swagger-output.json", import.meta.url), "utf-8")
);
app.use("/api-docs", swaggerUi.serve, swaggerUi.setup(swaggerDocument));
```

如果不是使用 ES6 則是加入：
```javascript
const express = require('express');
const app = express();

// swagger
const swaggerUi = require('swagger-ui-express');
const swaggerFile = require('./swagger-output.json'); // 引入剛生成的 JSON 文件

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerFile)); // 路徑可自訂，例如 /docs
```

伺服器啟動後，打開瀏覽器，輸入 `http://localhost:3000/api-docs` 你的互動式 API 文件就此誕生啦！🎉

### 4. 在路由中加入註解 📜

為了讓 swagger-autogen 能正確提取 API 資訊，會需要在路由檔案中撰寫註解，例如：

- **GET**
    ```javascript
    router.get(
      "/:userId",
      /* #swagger.tags = ['Cart'] */

      /* #swagger.description = "取得購物車所有商品資料" */

      /* #swagger.responses[200] = { 
          schema: {
            "status": 200,
            "message": "購物車成功取得資料",
            "data": {
                "cartItems": [
                    {
                        "id": 115,
                        "cart_id": 5,
                        "created_at": "2025-01-01T04:57:32.661Z",
                        "is_selected": false,
                        
        },
            description: "成功取得資料" } */

      CartController.fetchCartByUserId
    );
    ```
- **POST**
    ```javascript
    router.post(
  "/:userId",
      /* #swagger.tags = ['Cart'] */

      /* #swagger.description = "新增商品至購物車" */

      /*	#swagger.parameters['obj'] = {
                in: 'body',
                description: '新增商品',
                required: true,
                schema: {
                  "activityId": 34
                }
        } */

      /* #swagger.responses[201] = {
          schema: {
            "status": 201,
            "message": "成功新增至購物車",
            "data": {
                "id": 115,
                "cart_id": 5,
                "activity_id": 34,
                "created_at": "2025-01-01T04:57:32.661Z",
                "is_selected": false
            }
        },
            description: "成功新增至購物車" } */
        
      CartController.addActivityToCart
    );

    ```
註釋規範與格式可以前往作者的 GitHub 上參閱範例
>範例 📚 [Advanced Example](https://github.com/davibaltar/example-swagger-autogen-with-router)


### 5. 更新文件並測試 🔄

每次更新 API 或新增路由時，只需重新執行 node swagger.js，就能生成最新的 Swagger 文件！接著在 /api-docs 路徑中進行互動式測試，快速驗證 API 功能是否正常運作🚀。

