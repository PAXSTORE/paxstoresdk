
# PAXSTORE 3rd App Android SDK [![](https://jitpack.io/v/PAXSTORE/paxstore-3rd-app-android-sdk.svg)](https://jitpack.io/#PAXSTORE/paxstore-3rd-app-android-sdk)

PAXSTORE 3rd App Android SDK provides simple and easy-to-use service interfaces for third party developers to develop android apps on PAXSTORE. The services currently include the following features:

1. [Downloading parameter](docs/DownloadIntegration.md)
2. [Inquirer update for 3rd party app](docs/InstallInquirerIntegration.md)
3. [CloudMessage](docs/CloudMsgIntegration.md)
4. [GoInsight](docs/GoInsightIntegration.md)

By using this SDK, developers can easily integrate with PAXSTORE. Please take care of your AppKey and AppSecret that generated by PAXSTORE system when you create an app.
<br>Refer to the following steps for integration.


## Please check below mirgrations.md if you are using previous sdk.
[Migrations](docs/Migrations.md)


## Requirements
**Android SDK version**
>SDK 19 or higher, depending on the terminal's paydroid version.

**Gradle's and Gradle plugin's version**
>Gradle version 4.1 or higher  
>Gradle plugin version 3.0.0+ or higher

## Download
Gradle:

    implementation 'com.github.PAXSTORE:paxstore-3rd-app-android-sdk:8.0.1'


## Permissions
PAXSTORE Android SDK need the following permissions, please add them in AndroidManifest.xml.

`<uses-permission android:name="android.permission.INTERNET" />`<br>
`<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />`<br>
`<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />`<br>

## ProGuard
If you are using [ProGuard](https://www.guardsquare.com/en/products/proguard/manual) in your project add the following lines to your configuration:

    #Gson
    -dontwarn com.google.gson.**
    -keep class sun.misc.Unsafe { *; }
    -keep class com.google.gson.** { *; }
    -keep class com.google.gson.examples.android.model.** { *; }
    
    #JJWT
    -keepnames class com.fasterxml.jackson.databind.** { *; }
    -dontwarn com.fasterxml.jackson.databind.*
    -keepattributes InnerClasses
    -keep class org.bouncycastle.** { *; }
    -keepnames class org.bouncycastle.** { *; }
    -dontwarn org.bouncycastle.**
    -keep class io.jsonwebtoken.** { *; }
    -keepnames class io.jsonwebtoken.* { *; }
    -keepnames interface io.jsonwebtoken.* { *; }
    -dontwarn javax.xml.bind.DatatypeConverter
    -dontwarn io.jsonwebtoken.impl.Base64Codec
    -keepnames class com.fasterxml.jackson.** { *; }
    -keepnames interface com.fasterxml.jackson.** { *; }
    
    #dom4j
    -dontwarn org.dom4j.**
    -keep class org.dom4j.**{*;}
    -dontwarn org.xml.sax.**
    -keep class org.xml.sax.**{*;}
    -dontwarn com.fasterxml.jackson.**
    -keep class com.fasterxml.jackson.**{*;}
    -dontwarn com.pax.market.api.sdk.java.base.util.**
    -keep class com.pax.market.api.sdk.java.base.util.**{*;}
    -dontwarn org.w3c.dom.**
    -keep class org.w3c.dom.**{*;}
    -dontwarn javax.xml.**
    -keep class javax.xml.**{*;}
    
    #dto
    -dontwarn com.pax.market.api.sdk.java.base.dto.**
    -keep class com.pax.market.api.sdk.java.base.dto.**{*;}
    
    #By default Google keeps classes of  Activity 、Service ... 
    -keep public class * extends android.app.Activity
    -keep public class * extends android.app.Service
    -keep public class * extends android.content.BroadcastReceiver
    -keep public class * extends android.preference.Preference

## Set Up

### Step 1: Get Application Key and Secret
Create a new app in PAXSTORE, and get **AppKey** and **AppSecret** from app detail page in developer center.

### Step 2: Initialization
Configuring the application element, edit AndroidManifest.xml, it will have an application element. You need to configure the android:name attribute to point to your Application class (put the full name with package if the application class package is not the same as manifest root element declared package)

    <application
        android:name=".BaseApplication"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme">

Initializing AppKey,AppSecret and SN
>Please note, make sure you have put your own app's AppKey and AppSecret correctly

public class BaseApplication extends Application {

    private static final String TAG = BaseApplication.class.getSimpleName();
    
    //todo make sure to replace with your own app's appKey and appSecret
    private String appkey = "Your APPKEY";
    private String appSecret = "Your APPSECRET";
    
    @Override
    public void onCreate() {
        super.onCreate();
        initPaxStoreSdk();
    }
    
    private void initPaxStoreSdk() {
       //todo Init AppKey，AppSecret, make sure the appKey and appSecret is corret.
        StoreSdk.getInstance().init(getApplicationContext(), appkey, appSecret, new BaseApiService.Callback() {
            @Override
            public void initSuccess() {
               //TODO Do your business here
            }

            @Override
            public void initFailed(RemoteException e) {
               //TODO Do failed logic here
                Toast.makeText(getApplicationContext(), "Cannot get API URL from PAXSTORE, Please install PAXSTORE first.", Toast.LENGTH_LONG).show();
            }
        });
    }
}

## More Apis

#### [StoreSdk](docs/StoreSdk.md)

#### [ParamApi](docs/ParamApi.md)

#### [SyncApi](docs/SyncApi.md)

#### [UpdateApi](docs/UpdateApi.md)

#### [ResultCode](docs/ResultCode.md)


## Migrating to Android 8.0
Since Android 8.0 has lots of changes that will affect your app's behavior, we recommand you to follow the guide to migrate
to Android 8.0. For further information, you can refer to https://developer.android.google.cn/about/versions/oreo/android-8.0-migration


## FAQ

#### 1. How to resolve dependencies conflict?

When dependencies conflict occur, the error message may like below:

    Program type already present: xxx.xxx.xxx

**Solution:**

You can use **exclude()** method to exclude the conflict dependencies by **group** or **module** or **both**.

e.g. To exclude 'com.google.code.gson:gson:2.8.5' in SDK, you can use below:

    implementation ('com.pax.market:paxstoresdk:x.xx.xx'){
        exclude group: 'com.google.code.gson', module: 'gson'
    }

#### 2. How to resolve attribute conflict?

When attribute conflict occur, the error message may like below:

    Manifest merger failed : Attribute application@allowBackup value=(false) from 
    AndroidManifest.xml...
    is also present at [com.pax.market:paxstore-3rd-app-android-sdk:x.xx.xx] 
    AndroidManifest.xml...
    Suggestion: add 'tools:replace="android:allowBackup"' to <application> element
    at AndroidManifest.xml:..

**Solution:**

Add **xmlns:tools="http\://<span></span>schemas.android.com/tools"** in your manifest header

       <manifest xmlns:android="http://schemas.android.com/apk/res/android"
            package="com.yourpackage"
            xmlns:tools="http://schemas.android.com/tools">

Add **tools:replace = "the confilct attribute"** to your application tag:

        <application
            ...
            tools:replace="allowBackup"/>


More questions, please refer to [FAQ](https://github.com/PAXSTORE/paxstore-3rd-app-android-sdk/wiki/FAQ)

## License

See the [Apache 2.0 license](https://github.com/PAXSTORE/paxstore-3rd-app-android-sdk/blob/master/LICENSE) file for details.

    Copyright 2018 PAX Computer Technology(Shenzhen) CO., LTD ("PAX")
    
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at following link.
    
         http://www.apache.org/licenses/LICENSE-2.0
    
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
