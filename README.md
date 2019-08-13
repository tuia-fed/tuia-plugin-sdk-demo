# 推啊sdk集成文档
## 概述
插件化广告sdk，集成后可展示各类广告

## 使用说明
步骤一：引入aar

1)在工程的build.gradle添加远程仓库

```
repositories {
        google()
        jcenter()
        maven { url "https://raw.githubusercontent.com/tuia-fed/tuia-plugin-sdk/master" }
    }
```
```
allprojects {
    repositories {
        google()
        jcenter()
        maven { url "https://raw.githubusercontent.com/tuia-fed/tuia-plugin-sdk/master" }
    }
}
```

2)项目的build.gradle中添加依赖
```
implementation 'com.tuia.sdk:plugin_sdk:1.0.0'
```

步骤二：配置appKey
在项目的清单配置文件AndroidManifest.xml中添加
```
        <meta-data
            android:name="TUIA_APPKEY"
            android:value="应用的appKey" />
```

步骤三：sdk初始化
必须要在application中完成初始化

```
public class MyApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        TuiaSdk.getInstance().init(this);
    }
}
```
步骤四：在Activity中展示需求的相应类型广告
广告类view动态加入，需要在布局文件中预埋广告的viewgroup
```
    <FrameLayout
        android:id="@+id/viewGroup"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
```

```
   @Override
   protected void onCreate(Bundle savedInstanceState) {
      AdConfig adConfig = new AdConfig.Builder().setAdFoxType(AdConsts.SHOW_AD_TYPE_STREAMVIEW)
                .setAdFoxSize(AdConsts.FoxNa_750_180)
                .setAdslot_id(301969)
                .setFoxListener(new IAdFoxListener() {
                    @Override
                    public void onReceiveAd() {
                        Log.i(TAG,"================onReceiveAd=============");
                    }

                    @Override
                    public void onFailedToReceiveAd() {
                        Log.i(TAG,"================onFailedToReceiveAd=============");
                    }

                    @Override
                    public void onLoadFailed() {
                        Log.i(TAG,"================onLoadFailed=============");
                    }

                    @Override
                    public void onCloseClick() {
                        Log.i(TAG,"================onCloseClick=============");
                    }

                    @Override
                    public void onAdClick() {
                        Log.i(TAG,"================onAdClick=============");
                    }

                    @Override
                    public void onAdExposure() {
                        Log.i(TAG,"================onAdExposure=============");
                    }
                })
                .build();
        TuiaSdk.getInstance().showFoxAd(this,viewGroup,adConfig);
   }
   
```

步骤五：广告类回收：
在activity的onDestroy时，需要关闭广告的资源
```
    @Override
    protected void onDestroy() {
        TuiaSdk.getInstance().onDestroy(this);
        super.onDestroy();
    }
```

#接口

#### <div id='showFoxAd'>showFoxAd</div>
***
创建广告
```
TuiaSdk.getInstance().showFoxAd(this,viewGroup,adConfig);
```
##### 说明:
根据配置和需求创建相应的广告

##### 返回值：
无

##### 参数:
* this(Activity):上下文环境
* viewGroup(viewGroup):用于展示广告的布局
* adConfig([*AdConfig](#AdConfig)):用于展示广告的配置信息


#类
#### <div id='AdConfig'>AdConfig</div>
*** 广告配置类

* adslot_id :广告位Id
* userId:用户Id* 
* adFoxType:广告类型(详情见[*AdConsts](#AdConsts))
* adFoxSize:广告尺寸详情见[*AdConsts](#AdConsts))
* adFoxTime:广告展示时间(注：此参数仅用于启动页类广告)
* targetClass:启动页显示完后需跳转的目标类(注：此参数仅用于启动页类广告)
* IAdFoxShListener:启动页广告的回调函数(注：此参数仅用于启动页类广告的回调)
* IAdFoxListener:广告的回调函数
* IAdFoxNsTmListener:自定义广告的回调函数(注：此参数用于自定义类广告的回调)



#### <div id='AdConsts'>AdConsts</div>
*** 广告配置的常量类

#### adFoxType对应的广告类型

* SHOW_AD_TYPE_STREAMVIEW:信息流广告
* SHOW_AD_TYPE_WALL:应用墙，浮标类广告
* SHOW_AD_TYPE_SPLASH:启动页广告(注：1.0.0版本暂不支持)
* SHOW_AD_TYPE_TB_SCREEN:插屏类广告
* SHOW_AD_TYPE_CUSTOMER：自定义广告
* SHOW_AD_TYPE_BANNER:横幅类广告

#### adFoxSize对应的广告尺寸
* FoxNa_750_180:750*180大小，用于SHOW_AD_TYPE_STREAMVIEW广告
* FoxNa_750_420:750*420大小，用于SHOW_AD_TYPE_STREAMVIEW广告
* TMNA_640_150:640*150大小，用于SHOW_AD_TYPE_BANNER广告
* TMNA_640_280:640*280大小，用于SHOW_AD_TYPE_BANNER广告



