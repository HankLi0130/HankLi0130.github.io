---
title: "Android App 內部更新 (In-app updates)"
date: 2024-06-22T16:13:02+08:00
tags: ["Android"]
draft: false
image: ""
---

# Android In-app updates

Google Play 提供了 [App 內部更新](https://developer.android.com/guide/playcore/in-app-updates)，讓使用者除了到 Google Play 商店上做更新以外，現在也能夠在 App 內更新了．

這篇文章就來聊聊如何實作 Android in-app updates．

# 有哪些選擇？

Google 提供了兩種內部更新，立即更新 (immediate updates) 以及 彈性更新 (flexible updates)

這兩種的差別在使用者在按下更新鍵後能不能繼續操作 App．

## 彈性更新 (flexible update)

彈性更新可以讓使用者選擇先下載檔案，下載期間使用者可以繼續操作 App，完成下載後再進行更新．

如果您有一些小新功能想讓使用者嚐鮮，這種更新方式就很適合．

![flexible flow](/post/2024/0622_flexible_flow.png)

## 立即更新 (immediate update)

立即更新會將下載跟安裝流程一次完成，使用者在更新期間無法操作 App．

如果您有一些重要更新，立即更新會是個好選擇．

![immediate flow](/post/2024/0622_immediate_flow.png)