[&#8249; IAP Doc Home](./)

<h1>IAP 集成指南</h1>
<<[../../shared/-VERSION-/version.md]

## 集成
用如下命令来集成 SDKBOX IAP 插件,请确保您可以正常执行的 SDKBOX 安装器.
```bash
sdkbox import iap
```

<<[../../shared/notice.md]

<!--## Configuration
<<[../../shared/sdkbox_cloud.md]
<<[../../shared/remote_application_config.md]-->


### JSON 配置
SDKBOX 安装器会自动在您的工程中添加一个样例配置文件`sdkbox_config.json`.在您编译工程前,请修改里面的参数,用您自己的应用信息

现在给一个修改例子,您需要在[iTunes Connect](http://itunesconnect.apple.com)获取一个应用id,然后替换 `<put the product id for ios here>`,或者,在[Google Play Console](https://play.google.com/apps/publish)申请一个应用id,并替换`<put your googleplay key here>`
```json
"ios" :
{
    "iap":{
        "items":{
            "remove_ads":{
                "id":"<put the product id for ios here>"
            }
        }
    }
},
"android":
{
    "iap":{
        "key":"<put your googleplay key here>",
        "items":{
            "coins":{ // 这是可消耗商品
                "id":"<put the product id for android here>"
            },
            "remove_ads":{
                "id":"<put the product id for android here>",
                "type":"non_consumable" // 这是不可消耗商品
            },
            "subscription_item":{
                "id":"<put the product id for android here>",
                "type":"subs" // 这是订阅商品
            }
        }
    }
}
```

__注意__: sdkbox_config.json 中的 Goolge IAP key 是从 `Google Play Console` 中取到的, 请看如下截图:

![](../../imgs/google_licensing_iab.png)


<<[sdkbox-config-encrypt.md]

## 使用

<<[usage.md]

<<[promoting_iap.md]

<<[api-reference.md]

<<[manual_integration.md]

<<[manual_ios.md]

<<[manual_android.md]

<<[extra-step.md]
