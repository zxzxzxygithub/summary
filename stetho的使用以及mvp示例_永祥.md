## stetho的使用

### 添加依赖

    compile 'com.facebook.stetho:stetho:1.4.1'

    compile 'com.facebook.stetho:stetho-okhttp3:1.4.1'


### 在application初始化

        if (BuildConfig.DEBUG) {Stetho.initializeWithDefaults(this);}

### 设置拦截器，检测网络（以retrofit+okhttp为例）
    Retrofit.Builder retrofitBuilder = new Retrofit.Builder().baseUrl(ApiContants.BASE_URL)
                    .addConverterFactory(GsonConverterFactory.create());
    OkHttpClient.Builder builder = new OkHttpClient.Builder();
    builder.addNetworkInterceptor(new StethoInterceptor());
    OkHttpClient client = builder.build();
    retrofitBuilder.client(client);
    mRetrofit = retrofitBuilder
    .build();
### 在chome调试
	在chrome浏览器地址栏输入以下内容，然后选择设备和app进行调试
	chrome://inspect   
	
### 示例工程
[stethodemo](https://github.com/zxzxzxygithub/retrofitmvpeventbus.git)
    
    
    
    









