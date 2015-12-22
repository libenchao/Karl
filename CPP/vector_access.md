# vector的[] 和 at访问方法
今天再做一道题目的时候，发现了一个非常怪异的现象。vector如果下标超出了实际数组的范围，竟然不会报错。

比如：

    vector<vector<int> > matrix(10, vector<int>(10));
    for (int i = 0; i < 20; ++i) {
        cout << matrix[i].size() << endl;
    }
会输出结果：

    10
    10
    10
    10
    10
    10
    10
    10
    10
    10
    0
    0
    0
    0
    0
    0
    0
    0
    0
    0

天啊，让我惊呆了。

我去查了一下手册，发现上面对于这个方法的描述是这样的：

> Access element
> Returns a reference to the element at position n in the vector container.
> 
> A similar member function, vector::at, has the same behavior as this operator function, except that vector::at is bound-checked and signals if the requested position is out of range by throwing an out_of_range exception. 
> 
> Portable programs should never call this function with an argument n that is out of range, since this causes undefined behavior.

然后，我又去看了一下at的方法：
> Returns a reference to the element at position n in the vector.

> The function automatically checks whether n is within the bounds of valid elements in the vector, throwing an out_of_range exception if it is not (i.e., if n is greater or equal than its size). This is in contrast with member operator[], that does not check against bounds.


然后我们可以得到一个重要结论：vector的[]方法就是为了模拟数组的形式，而且为了效率，没有做边界检查。但是at方法就是一个完整的方法，会做边界检查。

但是呢，并没有什么卵用，虽然我觉得可以在某些场合用带边界检查的at。但是基本不会了。所以还是要自己保证[]的范围。而且，一定要注意，超出范围的程序行为是不可预期的。