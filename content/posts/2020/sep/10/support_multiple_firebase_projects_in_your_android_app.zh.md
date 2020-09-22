---
title: "Android 設定多 Firebase 專案"
date: 2020-09-10T18:00:00+08:00
tags: [Android, Firebase]
draft: false
---

你曾是否想過在 Android 專案中建立多種環境? 當我建立第一個 Firebase 專案時，我就想如何區分 Debug 和 Release 環境．這篇文章要跟各位分享，如何設定不同 Variants 來串接多個 Firebase 專案．

# 開始吧！

### 步驟 1: 為你的 variants 取名
第一步就是先取名，這個名字後面會一直用到．我取了 `development, release` 來當作 Variants 的名字．

### 步驟 2: 為各個 variants 建立 keystores
因為我想要的功能，Firebae 需要 SHA-1 驗證，所以我建立兩組 Keystore 各個 Variants．

建立 Keystore 非常簡單，開啟 Android Studio 接著按照以下方法

- 選擇 `Build` 然後 `Generate Signed Bundle / APK`.

![create key step 1](/images/2020/sep/10/create_key_step_1.png)

- 選擇其中一個, 在這裡我選擇 Android App Bundle．
![create key step 2](/images/2020/sep/10/create_key_step_2.png)

- 按下 `Create New...` 按鈕.
![create key step 3](/images/2020/sep/10/create_key_step_3.png)

- 輸入相關資訊然後按下 `OK` 按鈕.
![create key step 4](/images/2020/sep/10/create_key_step_4.png)

當完成建立 Keystore 之後，關閉視窗，我們不需要建立 Bundle 或是 APK．

### 步驟 3: 設定 build types

回到 Android 專案中，開啟 `build.gradle`，加入 `signingConfigs` 和 `buildTypes` 區塊，以下參考

```gradle
android {
    signingConfigs {
        development {
            storeFile file('/Users/hank/development.keystore')
            storePassword 'password'
            keyAlias 'androiddevelopmentkey'
            keyPassword 'password'
        }
        release {
            storeFile file('/Users/hank/release.keystore')
            storePassword 'password'
            keyAlias 'androidreleasekey'
            keyPassword 'password'
        }
    }
    ...

    defaultConfig {
        applicationId "dev.hankli.iamstar"
        ...
    }
    buildTypes {
        development {
            debuggable true
            applicationIdSuffix ".dev"
            versionNameSuffix "-dev"
            signingConfig signingConfigs.development
        }
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release
        }
    }
}
```

### 步驟 4: 建立 Firebase 專案

在 Android Studio 左側的 Gradle 中按下 `signingReport`，SHA-1 會出現在下方視窗

![create firebase step 1](/images/2020/sep/10/create_firebase_step_1.png)

現在移到 Firebase Console，新增專案後新增 Android 裝置

**注意** 這裡的 Android package name 必須為 `applicationId + applicationIdSuffix`，否則將會失敗．這裡我的 development variant 輸入 `dev.hankli.iamstar.dev`

![create firebase step 2](/images/2020/sep/10/create_firebase_step_2.png)

- 下載 `google-services.json` 檔案，回到 Android Studio 建立對應的資料夾，並將檔案移到資料夾內

![create firebase step 3](/images/2020/sep/10/create_firebase_step_3.png)

- 太棒了，你已經完成 development 的設定了，重複以上步驟來建立 release 的設定．

### 步驟 5: 切換 variants
 
大功告成! 你已經完成所有設定，現在你只需要在 `Build Variants` 切換 variants，就能切換 Firebase 專案了．

![switch variants](/images/2020/sep/10/switch_variants.png)

### 提醒: 設定 provider

假使你有在 AndroidManifest.xml 中使用 provider，請將 `android:authorities` 的值改為 `${applicationId}.provider`．

![set provider](/images/2020/sep/10/set_provider.png)

感謝你的閱讀，有任何問題歡迎與我聯絡！