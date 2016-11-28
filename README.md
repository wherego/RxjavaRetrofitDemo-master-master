# Retrofit+Rxjava+okhttp基本操作和统一处理的demo

![Preview](https://github.com/wzgiceman/RxjavaRetrofitDemo-string-master/blob/master/gif/retrofit_string.gif)


        1.Retrofit+Rxjava+okhttp基本使用方法
        2.统一处理请求数据格式
        3.统一的ProgressDialog和回调Subscriber处理
        4.取消http请求
        5.预处理http请求
        6.返回数据的统一判断
        7.失败后的retry处理
        8.RxLifecycle管理生命周期，防止泄露
        9.文件上传下载(支持多文件，断点续传)
        10.Cache数据持久化和数据库（greenDao）两种缓存机制

##添加相关引用
```java
    /*rx-android-java*/
    compile 'io.reactivex:rxjava:+'
    compile 'com.squareup.retrofit:adapter-rxjava:+'
    compile 'com.trello:rxlifecycle:+'
    compile 'com.trello:rxlifecycle-components:+'
    /*rotrofit*/
    compile 'com.squareup.retrofit2:retrofit:+'
    compile 'com.squareup.retrofit2:converter-gson:+'
    compile 'com.squareup.retrofit2:adapter-rxjava:+'
    compile 'com.google.code.gson:gson:+'
```

##代码使用
```java
    //    完美封装简化版
    private void simpleDo() {
          SubjectPost postEntity = new SubjectPost(simpleOnNextListener,this);
          postEntity.setAll(true);
          HttpManager manager = HttpManager.getInstance();
          manager.doHttpDeal(postEntity);
    }

    //   回调一一对应
    HttpOnNextListener simpleOnNextListener = new HttpOnNextListener<List<Subject>>() {
        @Override
        public void onNext(List<Subject> subjects) {
            tvMsg.setText("已封装：\n" + subjects.toString());
        }

        /*用户主动调用，默认是不需要覆写该方法*/
        @Override
        public void onError(Throwable e) {
            super.onError(e);
            tvMsg.setText("失败：\n" + e.toString());
        }
    }
```

* 初始化一个请求数据的对象继承BaseEntity对象，传递一个sub回调对象和context对象，设置请求需要的参数
* 通过单利获取一个httpmanger对象，触发请求
* 结果统一通过BaseEntity中的fun1方法判断，最后返回传递的sub对象中

##优化迭代
根据反馈及时更新和优化的过程，如果在使用过程中有任何问题，欢迎反馈给我！

##思路
详细思路可以可以参看我的博客：
[Rxjava+ReTrofit+okHttp深入浅出-终极封装](http://blog.csdn.net/column/details/13297.html)

##变种
在之前的封装1-5中我们都是通过传统的GsonConverterFactory自动解析，这样做确实很方便，用户能直接获取返回的对象，不用关心具体的转换，但是：这随之而来有很多的缺陷（虽然官网推荐这样使用）；
比如：无法使用其他第三发转换框架；泛型无法中间传递，封装无法统一处理缓存结果；回调信息无法统一处理..........
所以我们在享受它遍历的同时也被迫的要限制做很多的处理，限制我们的扩展！
介绍如何放弃GsonConverterFactory，直接返回String，扩展我们的封装！（封装的整体思想和之前的封装一样，所以不会有大的改动！）
[Rxjava+ReTrofit+okHttp深入浅出-终极封装变种](https://github.com/wzgiceman/RxjavaRetrofitDemo-string-master)

**如有帮助，欢迎start和follow后续会出更多好的开源技术**


