### 拷贝文件
从插件安装包中的 `plugin/android/libs` 目录拷贝下列 __jar__ 文件到您的工程的 __proj.android/libs__ 目录。

> sdkbox.jar

> PluginShare.jar

> converter-gson-2.1.0.jar

> gson-2.7.jar

> okhttp-3.4.2.jar

> okio-1.9.0.jar

> picasso-2.5.2.jar

> retrofit-2.1.0.jar

> twitter-text-1.14.3.jar

> tweet-composer

> twitter-core


<<[../../shared/copy_jni_lib.md]


### 编辑 `AndroidManifest.xml`
在标签 __application tag__ 上添加下列权限：

```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

```

* 并且在标签 __application tag__ 上添加添加下列内容：

```xml
<activity
    android:name="com.twitter.sdk.android.core.identity.OAuthActivity"
    android:configChanges="orientation|screenSize"
    android:excludeFromRecents="true"
    android:exported="false" />

<service
    android:name="com.twitter.sdk.android.tweetcomposer.TweetUploadService"
    android:enabled="true"
    android:exported="false" />
```

### 编辑 `Android.mk`
编辑 `proj.android/jni/Android.mk`：

为 __LOCAL_WHOLE_STATIC_LIBRARIES__ 添加额外的库：
```
LOCAL_WHOLE_STATIC_LIBRARIES += android_native_app_glue
LOCAL_LDLIBS += -landroid
LOCAL_LDLIBS += -llog
LOCAL_WHOLE_STATIC_LIBRARIES += PluginShare
LOCAL_WHOLE_STATIC_LIBRARIES += sdkbox
```

在所有 __import-module__ 语句之前添加一条 call 语句：
```
$(call import-add-path,$(LOCAL_PATH))
```

在最后添加额外的 __import-module__ 语句：
```
$(call import-module, ./sdkbox)
$(call import-module, ./pluginshare)
```

这意味着您的语句顺序看起来像是这样：
```
$(call import-add-path,$(LOCAL_PATH))
$(call import-module, ./sdkbox)
$(call import-module, ./pluginshare)
```

### 编辑 `Application.mk`
编辑 `proj.android/jni/Application.mk`：

为 __APP_PATFORM__ 添加版本：
```
APP_PLATFORM := android-9
```
