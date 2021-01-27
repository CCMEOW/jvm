# 垃圾回收器

[Young GC和Full GC触发条件](https://www.zhihu.com/question/41922036/answer/93079526)
* Young GC：当young gen中的eden区分配满的时候触发。注意young GC中有部分存活对象会晋升到old gen，所以young GC后old gen的占用量通常会有所升高。
* Full GC：  
    * 当准备要触发一次young GC时，如果发现统计数据说之前young GC的平均晋升大小比目前old gen剩余的空间大，则不会触发young GC而是转为触发full GC（因为HotSpot VM的GC里，除了CMS的concurrent collection之外，其它能收集old gen的GC都会同时收集整个GC堆，包括young gen，所以不需要事先触发一次单独的young GC）；
    * 或者，如果有perm gen的话，要在perm gen分配空间但已经没有足够空间时，也要触发一次full GC；
    * 或者System.gc()、heap dump带GC，默认也是触发full GC。