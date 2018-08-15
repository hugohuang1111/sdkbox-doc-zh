### 拷贝文件
* 从插件安装包中的 `plugin/android/libs` 目录拷贝下列 __jar__ 文件到您的工程的 __proj.android/libs__ 目录。

> PluginBee7.jar

> sdkbox.jar


<<[../../shared/copy_jars.md]

<<[../../shared/copy_jni_lib.md]


* 从 `plugin/android/libs` 拷贝 `bee7-android-sdk-gamewall` 目录到您游戏工程目录的 `proj.android/libs/` 目录.


### 编辑 `AndroidManifest.xml`
在标签 __application tag__ 上添加下列权限：

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

为了 game wall 的视频能正常工作, 确保在您的游戏中有以下其一设置:

  - `<uses-sdk>` 的 targetSdkVersion 属性设置为最新的 Android SDK 版本

    ```xml
    <uses-sdk android:minSdkVersion="9" android:targetSdkVersion="23" .../>
    ```

  或者

  - 使 __hardware acceleration__ 属性生效。

    ```xml
        <application android:label="@string/app_name"
                 android:icon="@drawable/icon"
                 android:hardwareAccelerated="true">
        </application>
    ```

在接近文件底部的位置，拷贝并且粘贴下列定义到 __application tags__ 标签结尾处。

```xml
<activity>
  <intent-filter>
      <action android:name="android.intent.action.VIEW"/>
      <category android:name="android.intent.category.DEFAULT"/>
      <category android:name="android.intent.category.BROWSABLE"/>
      <data android:scheme="your_bee7_scheme" android:host="publisher"/>
  </intent-filter>
</activity>

<service
  android:name="com.bee7.sdk.service.RewardingService"
  android:process=":rewardingservice"
  android:enabled="true">
</service>

<receiver android:name="com.bee7.sdk.publisher.RewardReceiver" android:enabled="true" android:exported="true">
    <intent-filter>
        <action android:name="com.bee7.action.REWARD" />
    </intent-filter>
</receiver>

<receiver
  android:name="com.bee7.sdk.service.RewardingServiceReceiver"
  android:enabled="true"
  android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.USER_PRESENT" />
    </intent-filter>
</receiver>
```

### 编辑 `Android.mk`
编辑 `<project_root>/jni/Android.mk` :

为 __LOCAL_STATIC_LIBRARIES__ 添加额外的库：
```
LOCAL_WHOLE_STATIC_LIBRARIES += PluginBee7
LOCAL_WHOLE_STATIC_LIBRARIES += sdkbox
```

在所有 __import-module__ 语句之前添加一条 call 语句：
```
$(call import-add-path,$(LOCAL_PATH))
```

在最后添加额外的 __import-module__ 语句：
```
$(call import-module, ./sdkbox)
$(call import-module, ./pluginbee7)
```

这意味着您的语句顺序看起来像是这样：
```
$(call import-add-path,$(LOCAL_PATH))
$(call import-module, ./sdkbox)
$(call import-module, ./pluginbee7)
```

  __Note:__ 如果您使用的是 cocos2d-x 预编译库，那么保证这些语句在已有的 `$(call import-module,./prebuilt-mk)` 语句之上非常重要。

### 编辑 `Application.mk` （只限 Cocos2d-x v3.0 到 v3.2 版本）
* 编辑 `proj.android/jni/Application.mk` 保证 __APP_STL__ 的定义正确。如果 `Application.mk` 包含了 `APP_STL := c++_static` 语句，那么这条语句应该被改为：
```
APP_STL := gnustl_static
```

* 编辑 `proj.android/jni/Application.mk` to:

为 __APP_PATFORM__ 添加版本：
```
APP_PLATFORM := android-21
```

### 编辑 `project.property`

```
android.library.reference.1=./libs/bee7-android-sdk-gamewall
```
