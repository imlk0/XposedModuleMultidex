# XposedModuleMultidex
MultiDex for Xposed module app
能够解决`Xposed module`被加载时面临的`multidex`加载问题。

# 源码
在`Android MultiDex`库的基础上修改而来
Android Multi Dex Library » 1.0.3
[http://mvnrepository.com/artifact/com.android.support/multidex/1.0.3](http://mvnrepository.com/artifact/com.android.support/multidex/1.0.3)

# 缘由
在`dalvik`虚拟机上，默认不支持`multidex`，而google的`Multidex`解决方案有一定的局限性，在`Xposed`模块上，显然，入口类不是`Application`。
如果要自己实现一套加载方式，那会非常麻烦，所以我基于google的`MultiDex`库稍作修改，让它适用于`Xposed`模块

# 用法

添加依赖
```
  compile 'top.imlk.xpmodulemultidex:XposedModuleMultidex:1.0.0'
```

然后在入口类的`handleLoadPackage`中执行
```
...
    @Override
    public void handleLoadPackage(LoadPackageParam lpparam) throws Throwable {
        XMMultiDex.install(ReverseXposedModule.class.getClassLoader(),MODULE_PATH,lpparam.appInfo);
...
```
三个参数分别为：模块的ClassLoader，模块安装包的绝对路径，宿主app的ApplicationInfo

其中`ReverseXposedModule`就是我这个模块的入口类的名称啦。

通过给入口类继承`IXposedHookZygoteInit`并实现`initZygote`方法
```
    @Override
    public void initZygote(StartupParam startupParam) throws Throwable {
        MODULE_PATH = startupParam.modulePath;
    }
```
就能拿到模块的安装包路径啦。

# LICENSE
Apache License, Version 2.0

