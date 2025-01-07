---
date: "2025-01-03T18:59:35+08:00"
draft: false
title: "使用 Git 安裝並設置 Hugo 主題 🛠️"
---
這裡使用的是  https://blowfish.page/zh-cn/ 🐡

也可以前往  https://themes.gohugo.io/ 尋找更多主題 ✨

## 使用 git 來安裝

 [如何搭建 Hugo 網站](https://www.notion.so/Hugo-16f30d8101ec80b5b8d2f2766e169b0a?pvs=21) 

需先將剛剛建立的 Hugo 專案推送至 git repo

1. 進入剛才創建的網站目錄 **`mywebsite`** ，將 Blowfish 新增為子模組
    
    ```bash
    cd mywebsite
    git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
    ```
    

1. 將根目錄裡存在的 **`hugo.toml`** 檔案刪除
    - 因為之後都會用到這個主題的 .toml 檔案來跑，如果不刪掉預設的檔案的話本地伺服器會無法 run 起來

2. 輸入下面指令在本地伺服器測試是否有問題
    
    ```bash
     hugo server -t blowfish //這裡是主題名稱可替換
    ```
    

## 更新主題

可以使用 **`git`** 指令更新，只要執行以下指令，最新版的主題將會下載到本地倉庫

```bash
git submodule update --remote --merge
```