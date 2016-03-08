# memcpy and memmove
这两个函数是用来进行内存拷贝的。其原型为：
```void * memmove ( void * destination, const void * source, size_t num );```
```void * memcpy ( void * destination, const void * source, size_t num );```
可以发现，两个函数的原型声明是一样的。都是用来进行内存拷贝的。拷贝的数量由用户来保证。拷贝是没有类型的，也就是进行的是byte级别的拷贝。

而他们的区别是，如果src和des两段内存有重复的话，只能用memmove了。

memcpy的实现，会对有重叠区域的情况报错；如果没有重叠区域，然后就一次拷贝。

对于有重叠的情形，memmove会进行判断，如果是des的前半段和src的后半段有重叠，则从后往前拷贝；else则从前往后拷贝。