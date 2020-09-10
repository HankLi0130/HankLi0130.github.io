---
title: "Support multiple environments in your android app"
date: 2020-09-10
tags: [Android, Firebase]
draft: false
---

Have you ever thought about create multiple environments in Android app? I'd got this question since I created my first Firebase project and finally I tried and found the answer these days. This artical is sharing how to make different variants to connect to different Firebase projects and switch them easily in Android projects.

# Let's get it started

### Step 1: Name variants
Firstable, give names for your variants. For instance, I have two variants and named them as `development, release`.

### Step 2: Create keystores for each variants
I created two keystores for my development and release variants because Firebase need SHA-1 certification to enable the functionalities I need.

It's really easy to create a keystore, Just open Android Studio and follow steps below to create keys.

- Choose `Build` and `Generate Signed Bundle / APK`.

![create key step 1](/images/2020/sep/10/create_key_step_1.png)

- Choose one of these two options, in my case, I selected the Android App Bundle.
![create key step 2](/images/2020/sep/10/create_key_step_2.png)

- Click `Create New...` button.
![create key step 3](/images/2020/sep/10/create_key_step_3.png)

- Type the informations and then press `OK` button.
![create key step 4](/images/2020/sep/10/create_key_step_4.png)

When you created the keys, close the window because we don't need create Bundle or APK.

### Step 3: Configure build types

Add `signingConfigs` block and `buildTypes` block like below and change to your settings.

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

### Step 3: Create Firebase projects

- Click `signingReport` task and the SHA-1 will show up on the window.

![create firebase step 1](/images/2020/sep/10/create_firebase_step_1.png)

- Move to Firebase Console, create projects and add Firebase to your Android app. 
**Note** Here the Android package name must be typed in `applicationId + applicationIdSuffix`, otherwise it won't work. Here I typed `dev.hankli.iamstar.dev` for my development variant.

![create firebase step 2](/images/2020/sep/10/create_firebase_step_2.png)

- Download `google-services.json` file, create specific folder and move the file to your project.

![create firebase step 3](/images/2020/sep/10/create_firebase_step_3.png)

- And the same steps with the release one.

### Step 4: Switch the variants
 
Congratulations! You're done all the settings, just switch your variants at `Build Variants` window.

![switch variants](/images/2020/sep/10/switch_variants.png)

### Optional: Setting provider

If you're using provider in AndroidManifest.xml, rename the package name to `${applicationId}.provider` to `android:authorities` property.

![set provider](/images/2020/sep/10/set_provider.png)