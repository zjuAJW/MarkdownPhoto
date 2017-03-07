#Mongo 	内存使用  
# 
Mongo采用的是内存映射存储引擎， 它会把数据文件映射到内存中，如果是读操作，内存中的数据起到缓存的作用，如果是写操作，内存还可以把随机的写操作转换成顺序的写操作，总之可以大幅度提升性能。MongoDB并不干涉内存管理工作，而是把这些工作留给操作系统的虚拟内存管理器去处理，这样做的好处是简化了MongoDB的工作，但坏处是你没有方法很方便的控制MongoDB占多大内存。   

默认情况下，WiredTiger引擎会使用所有空闲的内存，所以mongo占用内存可能会非常高。官方文档：

>	Starting in 3.4, the WiredTiger internal cache, by default, will use the larger of either:  
>	1.   50% of RAM minus 1 GB, or  
>	2.   256 MB.  
>	Via the filesystem cache, MongoDB automatically uses all free memory that is not used by the WiredTiger cache >
>	or by other processes. Data in the filesystem cache is compressed.

但是在公司测试机上测试时，插入的数据大小应该是大于测试机的内存了，但是最终内存只占了44.3%左右，这是个问题，不知道是不是系统自身对进程的有限制。   

最头疼的是，Mongo自己不释放内存，一直占用着很高，网上目前找到的方法都没法用，只能重启mongo服务，但是这样不解决根本问题，再次对collection进行操作，理论上还是会读取数据进到内存，所以对于数据量很大的单个collection，有什么解决方案吗？