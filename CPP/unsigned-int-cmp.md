# C语言中的unsigned问题
不知道为什么C语言中有这么一个问题，难道是设计人员觉得unsigned的数据范围比int大，所以比较的时候将int的位模式解释为unsigned？

这个问题出现的情况如下：

1. 加减法的地方
        
        int sum (vector<int> &A) {
            int ret = 0;
            for (int i = 0; i <= A.size()-1; ++i) {
                sum += A[i];
            }
            return ret;
        }
在这段代码中，如果传进来的A是个空的，就会有问题。
2. 比较的地方
        
        int maxlen (vector<string> &A) {
            int len = -1;
            for (auto a : A) {
                if (a.size() > len) {
                    len = a.size();
                }
            }
            return len;
        }
在这段代码中，一定会有问题。因为string.size()返回的是unsigned的类型。然后跟len比较的时候，len就解释成了unsigned。结果就成了最大的unsigned了，所以一定会有问题。