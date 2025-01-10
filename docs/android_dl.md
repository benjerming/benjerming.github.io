# Android中动态库加载依赖的方式

当以任何方式（Java的`System.load()`,`System.loadLibrary()`,C的`dlopen()`,Python的`cdll()`）链接某个库`liba.so`后，动态库会映射到进程空间，此后任意一个依赖`liba.so`的库均已满足此`DT_NEEDED`，所以保底方案是显示链接这些基础库。

## Android 5.0(api==21)和Android 5.1(api==22)

* `LD_LIBRARY_PATH` 无效，但可以调用`libdl.so`中的
  * `void set_android_LD_LIBRARY_PATH(const char *path)`设置`LD_LIBRARY_PATH`(测试有效)
  * `void get_android_LD_LIBRARY_PATH(char *, size_t)`获取`LD_LIBRARY_PATH`(测试无效)
* `dlopen(name_or_path)`链接动态库时，SONAME以`name_or_path`为准，动态库中的`$SONAME`字段无效
* `$ORIGIN` 无效

## Android >5.1(api>22)

* `$ORIGIN` 有效
  * 使用链接器无法添加`$ORIGIN`
  * 使用`patchelf`可以添加，并且可用
