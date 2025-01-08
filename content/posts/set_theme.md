---
date: "2025-01-03T18:59:35+08:00"
draft: false
title: "使用 Git 安裝並設置 Hugo 主題 🛠️"
---
## 🎤 前言 

嗨嗨～歡迎來到 Hugo 的主題魔法世界！✨ Hugo 是一個超靈活又快速的靜態網站生成器，而主題就是讓網站瞬間「變身」的祕密武器！(๑•̀ㅂ•́)و✧

今天，我將帶你一步步完成 Hugo 主題的安裝與管理～我們會以 Blowfish 主題（🐡） 
為例，一起來試試用 Git 子模組 的方法吧！ヾ(≧▽≦*)o

還在找主題嗎？沒關係！快去 [Blowfish](https://blowfish.page/ ) 主題官方網站 或 [Hugo Themes](https://themes.gohugo.io/ ) 官方網站 探索更多驚喜吧！💖


## 🎨 為什麼用 Git 安裝主題？
1. 管理超方便！💾
主題可以獨立存在，輕鬆更新、切換，完全不影響你的專案！(￣▽￣)b
2. 協作超輕鬆！🤝
跟朋友或團隊合作時，直接同步子模組就搞定，大家都用同一個版本，超貼心！
3. 穩定又可靠！🔒
從官方倉庫拉取主題，完全不怕下載壞掉的檔案～安心又放心！(๑•̀ㅂ•́)و✧

## 🛠️ 如何用 Git 安裝主題 

### 步驟一：準備好你的 Hugo 專案

在開始之前，請確認你已完成以下幾件事：
1. 建立 Hugo 專案：如果還沒開始，可以參考 如何搭建 Hugo 網站。ヽ(✿ﾟ▽ﾟ)ノ
2. 推送到 Git 倉庫：檢查你的專案是否已初始化 Git，並做好初始提交喔

### 步驟二：安裝 Blowfish 主題 🐡

1. 進入網站目錄
    先進入你的 Hugo 專案目錄～
    ```bash
    cd mywebsite
    ```
    
2. 添加 Blowfish 主題
用以下指令將 Blowfish 主題 加入專案：
    ```bash
    git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
    ```
3. 刪掉預設的配置檔 hugo.toml
因為 Blowfish 主題自帶配置檔，我們需要刪除原本的 hugo.toml，以免衝突！✂️

    ```bash
    rm hugo.toml
    ```
<br/>

ヽ(*´∀`)ﾉ 搞定了！接下來我們來測試主題是不是正常運行吧～

### 步驟三：測試主題是否正常運行 🖥️
1. 啟動本地伺服器
用以下指令啟動 Hugo 的本地伺服器～📡
    ```bash
    hugo server -t blowfish
    ```
2. 預覽你的網站
打開瀏覽器，進入 http://localhost:1313。
是不是超美的主題！🎉 🎉 恭喜你，成功安裝 Blowfish 主題啦！(≧∇≦)ﾉ✨
    
## 🔄 如何更新主題 
只需一條指令，就能讓你的主題煥然一新～
```bash
git submodule update --remote --merge
```
💡 小提醒：
- 更新後記得檢查網站是否正常運行，必要時調整一下配置檔！( ˘▽˘)っ
- 如果遇到問題，可以查看主題的官方文檔幫助排查喔～ (｀・ω・´)b

## 進階操作：切換或管理多主題 🎨
如果你有多個主題想嘗試，或者想切換主題，完全沒問題！✨

1. 添加另一個主題
    ```bash
    git submodule add https://github.com/other-theme-repo.git themes/other-theme
    ```
2. 切換主題進行測試
    ```bash
    hugo server -t other-theme
    ```
3. 刪除不需要的主題
    ```bash
    git submodule deinit -f themes/other-theme
    rm -rf .git/modules/themes/other-theme
    ```
<br/>
    
這樣就能輕鬆管理多個主題啦～( ੭˙ᗜ˙)੭、

## ✨ 結語 

恭喜你！現在已經學會用 Git 安裝並管理 Hugo 主題啦！(๑>ᴗ<๑)🎉
不管是初學者還是進階用戶，這種方式都能讓你的主題管理變得簡單又高效！快去試試看更多有趣的主題吧！🌈

如果你還沒有逛過 Hugo Themes 官方網站，千萬不要錯過！那裡有上百個主題等你挑選～(´▽`)ﾉﾞ✨

🚀 讓我們一起打造出屬於自己的靜態網站王國吧！ ヾ(≧▽≦*)o