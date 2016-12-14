## android全局变量解决方案

- 在application中设置一个私有变量，通过get和set方法获取和设置这个变量的值
- 外部获取的时候，需要先获取到这个application的实例，然后再进行get和set操作，例如

	 String name = ((MyApplication) getApplication()).getmName();
        Log.d("getname", "onStartCommand: name——" + name);
       
[参考demo](https://github.com/zxzxzxygithub/GlobalVariable/tree/master)