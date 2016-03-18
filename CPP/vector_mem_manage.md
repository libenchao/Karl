# vector内存管理

之前一直都知道vector是每次内存不够了，就直接申请当前内存的两倍。但是从来没有考虑其中存放的内容该怎么移动过去。

今天面试被问到了这个问题，我很理所当然的觉得应该是直接内存拷贝吧。而且还使劲解释了半天为什么是对的。

结果晚上回来自己动手做了一下发现，其实是调用了对象的拷贝构造函数来拷贝的，而且原来的对象会被析构掉。

附上测试代码：
    #include <iostream>
    #include <vector>
    
    using namespace std;
    
    class Test {
        int id;
    
    public:
        Test(int i): id(i) {
            cout << "constructing " << id << endl;
        }
    
        Test(const Test &t) {
            id = t.id;
            cout << "copy constructing " << id << endl;
        }
    
        Test & operator= (const Test t) {
            id = t.id;
            cout << "assign constructing " << id << endl;
            return *this;
        }
    
        ~Test() {
            cout << "destructing " << id << endl;
        }
    };
    
    int main() {
        vector<Test> v;
        for (int i = 0; i < 100; ++i) {
            v.push_back(Test(i));
        }
    
        return 0;
    }
    
其输出结果是：

```
constructing 0
copy constructing 0
destructing 0
constructing 1
copy constructing 1
copy constructing 0
destructing 0
destructing 1
constructing 2
copy constructing 2
copy constructing 1
copy constructing 0
destructing 1
destructing 0
destructing 2
constructing 3
copy constructing 3
destructing 3
constructing 4
copy constructing 4
copy constructing 3
copy constructing 2
copy constructing 1
copy constructing 0
destructing 3
destructing 2
destructing 1
destructing 0
destructing 4
constructing 5
copy constructing 5
destructing 5
constructing 6
copy constructing 6
destructing 6
constructing 7
copy constructing 7
destructing 7
constructing 8
copy constructing 8
copy constructing 7
copy constructing 6
copy constructing 5
copy constructing 4
copy constructing 3
copy constructing 2
copy constructing 1
copy constructing 0
destructing 7
destructing 6
destructing 5
destructing 4
destructing 3
destructing 2
destructing 1
destructing 0
destructing 8
constructing 9
copy constructing 9
destructing 9
constructing 10
copy constructing 10
destructing 10
constructing 11
copy constructing 11
destructing 11
constructing 12
copy constructing 12
destructing 12
constructing 13
copy constructing 13
destructing 13
constructing 14
copy constructing 14
destructing 14
constructing 15
copy constructing 15
destructing 15
constructing 16
copy constructing 16
copy constructing 15
copy constructing 14
copy constructing 13
copy constructing 12
copy constructing 11
copy constructing 10
copy constructing 9
copy constructing 8
copy constructing 7
copy constructing 6
copy constructing 5
copy constructing 4
copy constructing 3
copy constructing 2
copy constructing 1
copy constructing 0
destructing 15
destructing 14
destructing 13
destructing 12
destructing 11
destructing 10
destructing 9
destructing 8
destructing 7
destructing 6
destructing 5
destructing 4
destructing 3
destructing 2
destructing 1
destructing 0
destructing 16
constructing 17
copy constructing 17
destructing 17
constructing 18
copy constructing 18
destructing 18
constructing 19
copy constructing 19
destructing 19
constructing 20
copy constructing 20
destructing 20
constructing 21
copy constructing 21
destructing 21
constructing 22
copy constructing 22
destructing 22
constructing 23
copy constructing 23
destructing 23
constructing 24
copy constructing 24
destructing 24
constructing 25
copy constructing 25
destructing 25
constructing 26
copy constructing 26
destructing 26
constructing 27
copy constructing 27
destructing 27
constructing 28
copy constructing 28
destructing 28
constructing 29
copy constructing 29
destructing 29
constructing 30
copy constructing 30
destructing 30
constructing 31
copy constructing 31
destructing 31
constructing 32
copy constructing 32
copy constructing 31
copy constructing 30
copy constructing 29
copy constructing 28
copy constructing 27
copy constructing 26
copy constructing 25
copy constructing 24
copy constructing 23
copy constructing 22
copy constructing 21
copy constructing 20
copy constructing 19
copy constructing 18
copy constructing 17
copy constructing 16
copy constructing 15
copy constructing 14
copy constructing 13
copy constructing 12
copy constructing 11
copy constructing 10
copy constructing 9
copy constructing 8
copy constructing 7
copy constructing 6
copy constructing 5
copy constructing 4
copy constructing 3
copy constructing 2
copy constructing 1
copy constructing 0
destructing 31
destructing 30
destructing 29
destructing 28
destructing 27
destructing 26
destructing 25
destructing 24
destructing 23
destructing 22
destructing 21
destructing 20
destructing 19
destructing 18
destructing 17
destructing 16
destructing 15
destructing 14
destructing 13
destructing 12
destructing 11
destructing 10
destructing 9
destructing 8
destructing 7
destructing 6
destructing 5
destructing 4
destructing 3
destructing 2
destructing 1
destructing 0
destructing 32
constructing 33
copy constructing 33
destructing 33
constructing 34
copy constructing 34
destructing 34
constructing 35
copy constructing 35
destructing 35
constructing 36
copy constructing 36
destructing 36
constructing 37
copy constructing 37
destructing 37
constructing 38
copy constructing 38
destructing 38
constructing 39
copy constructing 39
destructing 39
constructing 40
copy constructing 40
destructing 40
constructing 41
copy constructing 41
destructing 41
constructing 42
copy constructing 42
destructing 42
constructing 43
copy constructing 43
destructing 43
constructing 44
copy constructing 44
destructing 44
constructing 45
copy constructing 45
destructing 45
constructing 46
copy constructing 46
destructing 46
constructing 47
copy constructing 47
destructing 47
constructing 48
copy constructing 48
destructing 48
constructing 49
copy constructing 49
destructing 49
constructing 50
copy constructing 50
destructing 50
constructing 51
copy constructing 51
destructing 51
constructing 52
copy constructing 52
destructing 52
constructing 53
copy constructing 53
destructing 53
constructing 54
copy constructing 54
destructing 54
constructing 55
copy constructing 55
destructing 55
constructing 56
copy constructing 56
destructing 56
constructing 57
copy constructing 57
destructing 57
constructing 58
copy constructing 58
destructing 58
constructing 59
copy constructing 59
destructing 59
constructing 60
copy constructing 60
destructing 60
constructing 61
copy constructing 61
destructing 61
constructing 62
copy constructing 62
destructing 62
constructing 63
copy constructing 63
destructing 63
constructing 64
copy constructing 64
copy constructing 63
copy constructing 62
copy constructing 61
copy constructing 60
copy constructing 59
copy constructing 58
copy constructing 57
copy constructing 56
copy constructing 55
copy constructing 54
copy constructing 53
copy constructing 52
copy constructing 51
copy constructing 50
copy constructing 49
copy constructing 48
copy constructing 47
copy constructing 46
copy constructing 45
copy constructing 44
copy constructing 43
copy constructing 42
copy constructing 41
copy constructing 40
copy constructing 39
copy constructing 38
copy constructing 37
copy constructing 36
copy constructing 35
copy constructing 34
copy constructing 33
copy constructing 32
copy constructing 31
copy constructing 30
copy constructing 29
copy constructing 28
copy constructing 27
copy constructing 26
copy constructing 25
copy constructing 24
copy constructing 23
copy constructing 22
copy constructing 21
copy constructing 20
copy constructing 19
copy constructing 18
copy constructing 17
copy constructing 16
copy constructing 15
copy constructing 14
copy constructing 13
copy constructing 12
copy constructing 11
copy constructing 10
copy constructing 9
copy constructing 8
copy constructing 7
copy constructing 6
copy constructing 5
copy constructing 4
copy constructing 3
copy constructing 2
copy constructing 1
copy constructing 0
destructing 63
destructing 62
destructing 61
destructing 60
destructing 59
destructing 58
destructing 57
destructing 56
destructing 55
destructing 54
destructing 53
destructing 52
destructing 51
destructing 50
destructing 49
destructing 48
destructing 47
destructing 46
destructing 45
destructing 44
destructing 43
destructing 42
destructing 41
destructing 40
destructing 39
destructing 38
destructing 37
destructing 36
destructing 35
destructing 34
destructing 33
destructing 32
destructing 31
destructing 30
destructing 29
destructing 28
destructing 27
destructing 26
destructing 25
destructing 24
destructing 23
destructing 22
destructing 21
destructing 20
destructing 19
destructing 18
destructing 17
destructing 16
destructing 15
destructing 14
destructing 13
destructing 12
destructing 11
destructing 10
destructing 9
destructing 8
destructing 7
destructing 6
destructing 5
destructing 4
destructing 3
destructing 2
destructing 1
destructing 0
destructing 64
constructing 65
copy constructing 65
destructing 65
constructing 66
copy constructing 66
destructing 66
constructing 67
copy constructing 67
destructing 67
constructing 68
copy constructing 68
destructing 68
constructing 69
copy constructing 69
destructing 69
constructing 70
copy constructing 70
destructing 70
constructing 71
copy constructing 71
destructing 71
constructing 72
copy constructing 72
destructing 72
constructing 73
copy constructing 73
destructing 73
constructing 74
copy constructing 74
destructing 74
constructing 75
copy constructing 75
destructing 75
constructing 76
copy constructing 76
destructing 76
constructing 77
copy constructing 77
destructing 77
constructing 78
copy constructing 78
destructing 78
constructing 79
copy constructing 79
destructing 79
constructing 80
copy constructing 80
destructing 80
constructing 81
copy constructing 81
destructing 81
constructing 82
copy constructing 82
destructing 82
constructing 83
copy constructing 83
destructing 83
constructing 84
copy constructing 84
destructing 84
constructing 85
copy constructing 85
destructing 85
constructing 86
copy constructing 86
destructing 86
constructing 87
copy constructing 87
destructing 87
constructing 88
copy constructing 88
destructing 88
constructing 89
copy constructing 89
destructing 89
constructing 90
copy constructing 90
destructing 90
constructing 91
copy constructing 91
destructing 91
constructing 92
copy constructing 92
destructing 92
constructing 93
copy constructing 93
destructing 93
constructing 94
copy constructing 94
destructing 94
constructing 95
copy constructing 95
destructing 95
constructing 96
copy constructing 96
destructing 96
constructing 97
copy constructing 97
destructing 97
constructing 98
copy constructing 98
destructing 98
constructing 99
copy constructing 99
destructing 99
destructing 99
destructing 98
destructing 97
destructing 96
destructing 95
destructing 94
destructing 93
destructing 92
destructing 91
destructing 90
destructing 89
destructing 88
destructing 87
destructing 86
destructing 85
destructing 84
destructing 83
destructing 82
destructing 81
destructing 80
destructing 79
destructing 78
destructing 77
destructing 76
destructing 75
destructing 74
destructing 73
destructing 72
destructing 71
destructing 70
destructing 69
destructing 68
destructing 67
destructing 66
destructing 65
destructing 64
destructing 63
destructing 62
destructing 61
destructing 60
destructing 59
destructing 58
destructing 57
destructing 56
destructing 55
destructing 54
destructing 53
destructing 52
destructing 51
destructing 50
destructing 49
destructing 48
destructing 47
destructing 46
destructing 45
destructing 44
destructing 43
destructing 42
destructing 41
destructing 40
destructing 39
destructing 38
destructing 37
destructing 36
destructing 35
destructing 34
destructing 33
destructing 32
destructing 31
destructing 30
destructing 29
destructing 28
destructing 27
destructing 26
destructing 25
destructing 24
destructing 23
destructing 22
destructing 21
destructing 20
destructing 19
destructing 18
destructing 17
destructing 16
destructing 15
destructing 14
destructing 13
destructing 12
destructing 11
destructing 10
destructing 9
destructing 8
destructing 7
destructing 6
destructing 5
destructing 4
destructing 3
destructing 2
destructing 1
destructing 0
```