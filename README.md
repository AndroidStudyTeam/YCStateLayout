### YCStateLayout状态切换
#### 1.关于Android界面切换状态的介绍
	* 怎样切换界面状态？有些界面想定制自定义状态？状态如何添加点击事件？下面就为解决这些问题！
	* 内容界面
	* 加载数据中
	* 加载数据错误
	* 加载后没有数据
	* 没有网络


#### 2.思路转变，抽取分离类管理几种状态
	* 以前做法：
		* 直接把这些界面include到main界面中，然后动态去切换界面，后来发现这样处理不容易复用到其他项目中，而且在activity中处理这些状态的显示和隐藏比较乱
		* 利用子类继承父类特性，在父类中写切换状态，但有些界面如果没有继承父类，又该如何处理
	* 现在做法：
		* 让View状态的切换和Activity彻底分离开，必须把这些状态View都封装到一个管理类中，然后暴露出几个方法来实现View之间的切换。
		* 在不同的项目中可以需要的View也不一样，所以考虑把管理类设计成builder模式来自由的添加需要的状态View


#### 3.关于该状态切换工具优点分析
	* 可以自由切换内容，空数据，异常错误，加载，网络错误等5种状态
	* 父类BaseActivity直接暴露5中状态，方便子类统一管理状态切换
  	* 倘若有些页面想定制状态布局，也可以自由实现，很简单

#### 4.使用方法
    * **代码引用：compile 'cn.yc:YCStateLib:1.1.5'**
    * 如下所示，具体可以直接参考代码
```
statusLayoutManager = StateLayoutManager.newBuilder(this)
            .contentView(R.layout.activity_content)
            .emptyDataView(R.layout.activity_emptydata)
            .errorView(R.layout.activity_error)
            .loadingView(R.layout.activity_loading)
            .netWorkErrorView(R.layout.activity_networkerror)
            .build();


/**
 * 点击重新刷新数据
 */
private void initEmptyDataView() {
    statusLayoutManager.showEmptyData();
    LinearLayout ll_empty_data = (LinearLayout) findViewById(R.id.ll_empty_data);
    ll_empty_data.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            initData();
            adapter.notifyDataSetChanged();
            showContent();
        }
    });
}

/**
 * 点击重新刷新
 */
private void initErrorDataView() {
    statusLayoutManager.showError();
    LinearLayout ll_error_data = (LinearLayout) findViewById(R.id.ll_error_data);
    ll_error_data.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            initData();
            adapter.notifyDataSetChanged();
            showContent();
        }
    });
}

/**
 * 点击设置网络
 */
private void initSettingNetwork() {
    statusLayoutManager.showNetWorkError();
    LinearLayout ll_set_network = (LinearLayout) findViewById(R.id.ll_set_network);
    ll_set_network.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            Intent intent = new Intent("android.settings.WIRELESS_SETTINGS");
            startActivity(intent);
        }
    });
}
```



#### 5.实现效果

![](https://github.com/yangchong211/YCStateLayout/blob/master/image/125771775308836257.png)
![](https://github.com/yangchong211/YCStateLayout/blob/master/image/407442243542773132.jpg)
![](https://github.com/yangchong211/YCStateLayout/blob/master/image/54463227589674008.png)
![](https://github.com/yangchong211/YCStateLayout/blob/master/image/739964693513198374.jpg)
![](https://github.com/yangchong211/YCStateLayout/blob/master/image/75707536091894445.jpg)


###  6.版本更新说明
- v1.0 更新于2017年3月28日
- v1.1 更新于2017年12月3日
- v1.1.5 更新于2018年4月25日



