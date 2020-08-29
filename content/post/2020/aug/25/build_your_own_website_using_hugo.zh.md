---
title: "使用 Hugo 建立自己的網站"
date: 2020-08-25T17:25:39+08:00
draft: false
---


![Hogo Logo](/image/2020/aug/25/hugo_logo.png)

# 為何選擇 Hugo?

身為開發者，我常需要紀錄我做過的東西，我發現 Hugo 很適合用來做部落格．接下來，我將跟各位分享如何使用 Hugo 建立網站以及部署到 GitHub 上

# 讓我們開始吧！

### 步驟 1: 安裝 Hugo

如果你使用 macOS 我建議使用 [Homebrew](https://brew.sh/) 來安裝 Hugo，因為非常簡單

開啟 Terminal 並輸入：
```text
> brew install hugo
```

### 步驟 2: 建立新網站

```text
> hugo new site [name]
```

這會建立一個新的 Hugo 網站在 `[name]` 資料夾裡
 
### 步驟 3: 安裝你喜歡的主題

去 [Hugo themes](https://themes.gohugo.io/) 挑一個你喜歡的主題並且複製該 GitHub 的網址

我使用 [Minimal](https://themes.gohugo.io/minimal/) 主題在我的部落格

```text
> cd [name]
> git init
> git submodule add [theme repository] themes/[theme name]
```

進入 `[name]` 資料夾並且執行 git 指令來安裝主題到你的部落格

### 步驟 4: 設定網站

根據主題的指示設定，通常你能夠在該主題網頁找到相關設定

這裡是 Minial 的設定說明 [here](https://themes.gohugo.io/minimal/#configuration)

### 步驟 5: 建立新文章

使用 Hugo 建立文章非常簡單，只需要以下指令

``` text
> hugo new posts/[post name].md
```

這會建立新文章在 `content` 資料夾，文章需要使用 markdown 撰寫

### 步驟 6: 測試網站

是時候測試網站了，輸入以下指令

```text
> hugo server -D
```

開啟遊覽器並輸入 `localhost:1313`

### 步驟 7: 部署到 GitHub

首先，在 GitHub 上建立 2 個 repository 並同步你的專案跟網站到上面

1. 在 GitHub 建立 `[YOUR-PROJECT]` (例如 blog) repository，這個 repository 包含 Hugo 的內容以及相關檔案.

2. 建立 `[USERNAME]`.github.io GitHub repository，這個 repository 包含使用 Hugo 產生的網站 (你的部落格)

3. 在你本機專案的 git 新增 remote 到你的 GitHub repository

4. 輸入以下指令

```text
> git submodule add -b master https://github.com/<USERNAME>/<USERNAME>.github.io.git public
```

5. 確定設定檔的 baseURL 指定到 `[USERNAME]`.github.io

6. 建立 [deploy.sh](https://gohugo.io/hosting-and-deployment/hosting-on-github/#put-it-into-a-script) 並執行以下指令

```text
> ./deploy.sh [commit message]
```

7. 打開瀏覽器輸入 `[USERNAME].github.io` 你將看到你的網站.

### 步驟 8: 享受你的網站

完成以上步驟，你能簡單的建立文章並且更新

# 參考

- [Quick Start](https://gohugo.io/getting-started/quick-start/)

- [Host on GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/)