#排序算法比较
排序算法有很多种，每种的使用场景也不是很相同，时间空间复杂度也不同。所以在这里对常见的排序算法进行一下总结归纳。

一般在实现真正的排序算法时，会选择一个渐进复杂度为O(nlogn)的算法，比如快排和归并。然而在小规模的数据上，还是使用插入排序这种算法比较合适，可以有效的降低函数调用栈的开销。

#冒泡排序
冒泡排序的思想很简单，就是如果两个相邻的元素的顺序不符合最终排序的顺序，则交换之。这样通过从前到后交换一轮之后，至少最大的数已经到了数组的最后端。然后经过n-1轮交换之后，就可以保证数组是有序的了。

算法的时间复杂度是O(n*n)，空间复杂度是O(1)。

冒泡排序应该是稳定的，因为我们在任何时候都不可能交换两个相同的元素。

    static void swap(int *x, int *y) {
        int tmp = *x;
        *x = *y;
        *y = tmp;
    }
    
    void bubble_sort(int *arr, int n) {
        for (int i = 1; i < n; ++i) {
            for (int *j = arr; j < arr+n-i; ++j) {
                if (*j > *(j+1)) {
                    swap(j, j+1);
                }
            }
        }
    }

## 插入排序
插入排序的思想是如果前面k个元素是有序的，可以将第k+1个元素插入到前面的k个元素中，使其变为有序。插入的时候从后往前检查第一个比k+1元素不大的数，还要注意是否到了左侧边界。

算法时间复杂度是O(n*n)，但是如果数组原来就是有序的，可以使得算法的复杂度降低到O(n)。空间复杂度是O(1)。

算法也是稳定的。因为我们可以在任何时候都不将后面的元素插入到前面与其相等的元素前面。

    static void swap(int *x, int *y) {
        int tmp = *x;
        *x = *y;
        *y = tmp;
    }
    
    void insert_sort(int *arr, int n) {
        for (int *i = arr+1; i < arr+n; ++i) {
            int *j = i;
            while (j > arr && *j < *(j-1)) {
                swap(j, j-1);
                j--;
            }
        }
    }

## 选择排序
选择排序的思想是每次在未排序的所有元素中选择最小的那个元素，使其与当前未排序元素的第一个进行交换。

算法的时间复杂度是O(n*n)。空间复杂度是O(1)。

选择排序应该是不稳定的。因为交换当前最小元素的时候，很有可能将第一个元素的位置进行了变换，使其到了与之相等元素的后面。

    static void swap(int *x, int *y) {
        int tmp = *x;
        *x = *y;
        *y = tmp;
    }
    
    void insert_sort(int *arr, int n) {
        for (int *i = arr; i < arr+n; ++i) {
            int *small = i;
            for (int *j = i+1; j < arr+n; ++j) {
                if (*j < *small) {
                    small = j;
                }
            }
            swap(i, small);
        }
    }

## 希尔排序
希尔排序是对插入排序的改进。由于插入排序中，如果后面的元素非常小的话，会跟前面的所有元素都比较一次。最坏的情况就是数组是倒序的。

希尔排序中，会选择一个h值，使得所有间隔为h的元素之间都是有序的。这样只要是一个最后为1的h序列，都可以将数组变为有序的。因为h为1可以使得希尔排序退化为插入排序。

希尔排序的时间复杂度理论上是小于插入排序的O(n*n),真正的复杂度非常难以分析，还要看选择的h序列。所以有人说希尔排序大约是O(n的1.25次方)。空间负责度是O(1)。

希尔排序是不稳定的。

    static void swap(int *x, int *y) {
        int tmp = *x;
        *x = *y;
        *y = tmp;
    }
    
    void shell_sort(int *arr, int n) {
        int h = 1;
        while (h < n/3) h = 3*h + 1;
    
        while (h >= 1) {
            for (int *i = arr+h; i < arr+n; ++i) {
                int *j = i;
                while (j >= arr+h && *j < *(j-h)) {
                    swap(j, j-h);
                    j -= h;
                }
            }
    
            h = h/3;
        }
    }

## 归并排序
归并排序是二分的非常好的例子。非常自然的想法，就是将一个数组分成两部分，然后递归进行排序，然后再将其合并。

归并排序的时间复杂度是O(nlogn)。但是空间复杂度是O(n)的。因为merge的时候需要一个额外的辅助空间。在网上看到有人说可以进行原地归并，这样就少了辅助空间了。采用的方法就是类似于字符串中进行移位的方法。[参考材料](http://www.cnblogs.com/daniagger/archive/2012/07/25/2608373.html)。不过我觉得，虽然这样是原地的归并了，但是复杂度好像是要上去了，因为这样的原地归并可以退化到O(n*n)的复杂度。

归并排序是稳定的。

    void merge(int *arr, int mid, int n) {
        int *buf = malloc(sizeof(int) * n);
    
        for (int i = 0; i < n; ++i) {
            *(buf+i) = *(arr+i);
        }
    
        int *left = buf, *right = buf+mid;
        for (int *i = arr; i < arr+n; ++i) {
            if (left >= buf+mid) *i = *right++;
            else if (right >= buf+n) *i = *left++;
            else if (*left < *right) *i = *left++;
            else *i = *right++;
        }
    }
    void merge_sort(int *arr, int n) {
        if (n < 2) return;
    
        int mid = n/2;
        merge_sort(arr, mid);
        merge_sort(arr+mid, n-mid);
    
        merge(arr, mid, n);
    }

## 快速排序
快速排序实际上也是基于分治的思想。只是分治之前先将数组调整为两部分，其中前部分的所有元素都小于等于后面部分的元素。这样就可以直接对两部分进行递归的快排就行了。

快速排序的时间复杂度在均摊情况下是O(nlogn)，而且比其他的同等时间复杂度的排序算法能够好的利用cache locality。所以快排的内部循环是非常高效的，所以一般用来实现库中的排序算法。但是如果分析最后的情况，快排会退化到O(n*n)的时间复杂度。快排的空间复杂度是O(1)。但是我感觉快排很难实现为非递归的形式，所以对于函数调用栈还是有一定的开销的。

快排也是不稳定的。

快速排序还存在很多种优化，其中一种就是三相快速排序。时间和空间复杂度都比两向的快速排序优。一开始是可以遍历数组找到中位数进行划分，后来是找三个数，取三者的中值进行划分，效率较好。

    int partition(int *arr, int n) {
        int mid = *arr;
    
        int *left = arr+1, *right = arr+n-1;
        while (left < right) {
            while (left < right && *left <= mid) left++;
            while (left < right && *right > mid) right--;
    
            *arr = *left;
            *left = *right;
            *right = *arr;
        }
    
        left--;
        *arr = *left;
        *left = mid;
    
        return left - arr;
    }
    
    void quick_sort(int *arr, int n) {
        if (n < 2) return;
    
        int mid = partition(arr, n);
        quick_sort(arr, mid);
        quick_sort(arr+mid+1, n-mid-1);
    }

## 堆排序
堆排序是通过将数组维护为一个堆，然后不断的缩小堆的大小来进行排序的。

堆排序的时间复杂度是O(nlogn)，而且建堆的过程本身就是O(n)的，调整堆的过程是O(logn)的。然后空间复杂度是O(1)的。

堆排序是不稳定的。

    void swap(int *x, int *y) {
        int tmp = *x;
        *x = *y;
        *y = tmp;
    }
    
    void sink(int *arr, int s, int n) {
        int son = (s << 1) + 1;
        while (son < n) {
            if (son + 1 < n && *(arr+son+1) > *(arr+son)) son++;
            if (*(arr+s) >= *(arr+son)) return;
            swap(arr+s, arr+son);
            s = son;
            son = (s << 1) + 1;
        }
    }
    
    void heap_sort(int *arr, int n) {
        for (int i = n/2; i >= 0; --i) {
            sink(arr, i, n);
        }
    
        for (int i = n-1; i > 0; --i) {
            swap(arr, arr+i);
            sink(arr, 0, i);
        }
    }