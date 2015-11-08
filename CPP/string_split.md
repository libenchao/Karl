# C++ string split 实现
C++中没有像java、或者python中那么方便的string的split函数，的确是一个很大的令人不爽的地方。经常会遇到这种需求，今天在做google的oj的时候就遇到的了这个问题，如果是用java的话，一句话就搞定了。不过我还是写了好多代码专门来转化字符串了。

回头查了一下，找到了一个实现起来比较方便的方式。不过不知道这种方式的效率如何。现在将其记录在这里。

    #include <string>
    #include <iostream>
    #include <vector>
    #include <sstream>
    
    using namespace std;
    
    vector<string> split(string input, char delim) {
        stringstream ss(input);
        vector<string> ans;
        string cur;
        while (getline(ss, cur, delim)) {
            ans.push_back(cur);
        }
        return ans;
    }
以后就可以用这种方式快速的做到字符串的split了。