#排序算法比较
#冒泡排序
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
