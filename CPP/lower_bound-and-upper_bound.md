# lower_bound 和 upper_bound函数的真正含义

C++中比较奇怪的两个函数。在set，map等有顺序的容器中存在的方法。  
一开始我只看了lower_bound的方法的解释，然后就感觉upper_bound恰好是找不大于x的第一个数。结果用起来完全不是这个意思。然后一看equal_range也完全不是想象中的意思。  
所以我是感觉几个函数的名字或者功能有点奇怪。

**然后,如果真的想实现不大于x的第一个数的功能的话，可以用lower_bound的返回值进行减一**  
**值得注意的是end也是可以减一的**

## lower_bound()
以下是lower_bound的ref的解释：
> Returns an iterator pointing to the first element in the container which is not considered to go before val (i.e., either it is equivalent or goes after).
> 
> The function uses its internal comparison object (key_comp) to determine this, returning an iterator to the first element for which key_comp(element,val) would return false.
> 
> If the set class is instantiated with the default comparison type (less), the function returns an iterator to the first element that is not less than val.
> 
> A similar member function, upper_bound, has the same behavior as lower_bound, except in the case that the set contains an element equivalent to val: In this case lower_bound returns an iterator pointing to that element, whereas upper_bound returns an iterator pointing to the next element.

中文意思就是找到第一个大于等于x的元素的指针。如果找不到，就返回end

## upper_bound()
以下是upper_bound的ref的解释：
> Returns an iterator pointing to the first element in the container which is considered to go after val.
> 
> The function uses its internal comparison object (key_comp) to determine this, returning an iterator to the first element for which key_comp(val,element) would return true.
> 
> If the set class is instantiated with the default comparison type (less), the function returns an iterator to the first element that is greater than val.
> 
> A similar member function, lower_bound, has the same behavior as upper_bound, except in the case that the set contains an element equivalent to val: In this case lower_bound returns an iterator pointing to that element, whereas upper_bound returns an iterator pointing to the next element.

然后翻译成中文的意思就是寻找第一个大于x的元素的指针。如果找不到，就返回end。

## equal_range()
以下是ref上对于equal_range的解释：
> Returns the bounds of a range that includes all the elements in the container that are equivalent to val.
> 
> Because all elements in a set container are unique, the range returned will contain a single element at most.
> 
> If no matches are found, the range returned has a length of zero, with both iterators pointing to the first element that is considered to go after val according to the container's internal comparison object (key_comp).
> 
> Two elements of a set are considered equivalent if the container's comparison object returns false reflexively (i.e., no matter the order in which the elements are passed as arguments).

翻译成中文就是相当于是，返回的是一个pair，first指向的是第一个等于x的元素，second指向的是第一个大于x的元素。其实相当与时lower_bound和upper_bound分别返回的结果。

<2015年9月22日>