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
        <meta-data
            android:name="TUIA_APPKEY"
            android:value="应用的appKey" />


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







