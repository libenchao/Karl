# shift in C/C++

shift in C++ not allow larger than 31. If you write such a expression, you will get the answer with ```%32```. For example:

    int x = 33;
	cout << (0xffffffff << x) << endl;
you will get the ans ```0xffffffff << 1```.

**But** if you write x in a constant expression like ```0xffffffff << 33```, the compiler will warn you. Buf finally it will do it for you. Then you will get the answer ```0xffffffff << 33``` and it's ```0```.

So it maybe make you confused, but it is. You can remenber there is no actual shift in C/C++ larger than 31, the reason you can shift more 31 using constant is a compiler work. It will do it for you in compile stage.