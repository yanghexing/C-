### 排序算法

#### 冒泡排序(bubble_sort)

冒泡排序算法的运作如下：

1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
3. 针对所有的元素重复以上的步骤，除了最后一个。
4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

实现代码：

```c++
void bubblesort(vector<int>&a){
    int len = a.size();
    for(int i = 0;i < len-1;i++){
        for(int j = 0;j < len-1-i;j++){
            if(a[j] > a[j+1])
                swap(a[j],a[j+1]);
        }
    }
}
```

**助记码**

```c++
i∈[0,N-1)               //循环N-1遍
   j∈[0,N-1-i)           //每遍循环要处理的无序部分
     swap(j,j+1)          //两两排序（升序/降序）
```

#### 选择排序(selection_sort)

**选择排序**（Selection sort）是一种简单直观的排序算法。它的工作原理如下。首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

选择排序的**主要优点与数据移动有关**。如果某个元素位于正确的最终位置上，则它不会被移动。选择排序每次交换一对元素，它们当中至少有一个将被移到其最终位置上，因此**对n个元素的表进行排序总共进行至多n-1次交换**。

实现代码：

```c++
void selection_sort(vector<int> &a){
    int len = a.size();
    for(int i = 0; i < len-1;i++){
        int min = i;
        for(int j = i+1;j < len;j++){
            if(a[j]<a[min])
                min = j;
        }
        swap(a[i],a[min]);
    }
}
```

#### 插入排序（insertion_sort)

插入排序和打扑克牌类似，例如：

Input: {5 2 4 6 1 3}。

首先拿起第一张牌, 手上有 {5}。

拿起第二张牌 2, 把 2 insert 到手上的牌 {5}, 得到 {2 5}。

拿起第三张牌 4, 把 4 insert 到手上的牌 {2 5}, 得到 {2 4 5}。

以此类推。

实现代码：

```c++
void insertion_sort(int arr[],int len){
        for(int i=1;i<len;i++){
                int key=arr[i];
                int j=i-1;
                while((j>=0) && (key<arr[j])){
                        arr[j+1]=arr[j];
                        j--;
                }
                arr[j+1]=key;
        }
}
```

#### 快速排序(quick_sort)

快速排序思想来源于冒泡排序，冒泡排序是通过相邻元素的比较和交换把最小的冒泡到最顶端，而快速排序是比较和交换小数和大数，这样一来不仅把小数冒泡到上面同时也把大数沉到下面。

举个例子：对5,3,8,6,4这个无序序列进行快速排序，思路是右指针找比基准数小的，左指针找比基准数大的，交换之。

5,3,8,6,4 用5作为比较的基准，最终会把5小的移动到5的左边，比5大的移动到5的右边。

5,3,8,6,4 首先设置i,j两个指针分别指向两端，j指针先扫描（思考一下为什么？）4比5小停止。然后i扫描，8比5大停止。交换i,j位置。

5,3,4,6,8 然后j指针再扫描，这时j扫描4时两指针相遇。停止。然后交换4和基准数。

4,3,5,6,8 一次划分后达到了左边比5小，右边比5大的目的。之后对左右子序列递归排序，最终得到有序序列。

**快速排序是不稳定的，其时间平均时间复杂度是`O(nlgn)`。**

实现代码：

```c++
static int quicsort_partition(vector<int>& a,int left,int right){
    int pivotKey = a[left];

    while(left < right){
        while(left < right && a[right] >= pivotKey)
            right --;
        a[left] = a[right];
        while(left < right && a[left] <= pivotKey)
            left++;
        a[right] = a[left];
    }
    a[left] = pivotKey;
    return left;
}

void quicksort(vector<int>& a,int left,int right){
    if(left >= right)
        return;
    int pivotPointet = quicsort_partition(a,left,right);
    quicksort(a,left,pivotPointet-1);
    quicksort(a,pivotPointet+1,right);
}
```

#### 堆排序(heap_sort)

堆排序是借助堆来实现的选择排序，思想同简单的选择排序。那么**什么是堆？**

1. 堆是一个**完全二叉树**。（完全二叉树特点： 生成顺序：**从上往下，从左往右依次生成**）
2. 堆满足所有的子节点都小于父节点。

首先实现堆排序需要解决几个问题：

1. 如何表示一个堆？
2. 如何将一个无序数组调整成一个堆？
3. 根据一个堆如何实现排序？

第一个问题，可以直接使用线性数组来表示一个堆。

![1566412981497](C:\Users\yanghexing\Desktop\算法\pic\vector_heap)

这样表示的好处是：对于任意一个节点i，我们可以通过计算得到它的父节点和子节点：
$$
parent = (i-1)/2
$$

$$
leftchild = 2*i+1
$$

$$
rightchild = 2*i+2
$$

第二个问题，怎么调整成堆？首先是将倒数第二层的节点和它的子节点调整成堆(`heapify`)，然后依次调整至根节点。

第三个问题，我们可以将最后一个节点和根节点进行交换，然后将除最后一个节点的剩余节点调整成堆，重复上述操作即可。

实现代码：

```c++
void heapify(int a[], int len, int i){
    int c1 = 2*i + 1;
    int c2 = 2*i + 2;
    int max = i;
    if(c1 < len && a[c1] > a[max])
        max = c1;
    if(c2 < len && a[c2] > a[max])
        max = c2;

    if(max != i){
       swap(a[i],a[max]);
       heapify(a,len, max);
    }
}

void build_heap(int a[],int len){
    int parent = (len - 1)/2;
    for(int i = parent;i >= 0;i--){
        heapify(a,len,i);
    }
}

void heap_sort(int a[], int len){
    build_heap(a, len);
    for(int i = len - 1;i >= 0;i--){
        swap(a[i],a[0]);
        heapify(a, i, 0);
    }
}
```

#### 归并排序(merge_sort)

#### 桶排序(bucket_sort)

#### 排序算法总结

|      排序方法      | 平均时间  | 最好时间 | 最坏时间 |
| :----------------: | :-------: | :------: | :------: |
|  冒泡排序（稳定）  |  O(n^2)   |   O(n)   |  O(n^2)  |
| 选择排序（不稳定） |  O(n^2)   |  O(n^2)  |  O(n^2)  |
|  插入排序（稳定）  |  O(n^2)   |   O(n)   |  O(n^2)  |
| 快速排序（不稳定） | O(nlogn)  | O(nlogn) |  O(n^2)  |
|  堆排序（不稳定）  | O(nlogn)  | O(nlogn) | O(nlogn) |
|  归并排序（稳定）  | O(nlogn)  | O(nlogn) | O(nlogn) |
| 希尔排序（不稳定） | O(n^1.25) |          |          |
|  基数排序（稳定）  |   O(n)    |   O(n)   |   O(n)   |
|  桶排序（不稳定）  |   O(n)    |   O(n)   |  O(n^2)  |

时间复杂度由小到大的顺序如下
$$
O(1) < O(logn) < O(n) < O(nlogn) < O(n^2) < O(n^3) < O(2^n) < O(n!) < O(n^n)
$$

### KMP算法

#### 部分匹配表：

|  pattern   |  A   |  B   |  A   |  B   |  C   |  A   |  B   |  A   |  A   |
| :--------: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| **index**  |  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |
| **prefix** |  -1  |  0   |  0   |  1   |  2   |  0   |  1   |  2   |  3   |

```c++
void prefix_table(string pattern,vector<int>& table){
    int len = pattern.length();
    int temp = 0;
    int i = 1;
    table.push_back(-1);
    table.push_back(temp);
    while(i<len-1){
        if(pattern[i] == pattern[temp]){
            table.push_back(++temp);
            i++;
        }else{
            temp = 0;
            if(pattern[i] != pattern[temp]){
                table.push_back(temp);
                i++;
            }
        }
    }
}

int kmpsearch(string text,string pattern,vector<int> prefix){
    int i = 0;
    int j = 0;
    while(i<text.length()&&j<pattern.length()){
        if(text[i] == pattern[j]){
            j++;i++;
        }
        else{
            j = prefix[j];
            if(j == -1){
                j++;i++;
            }
        }
    }
    if(j == pattern.length())
        return i-j;
    else
        return -1;
}


int main()
{
    string text = "ABCABABCABAAD";
    string pattern = "ABABCABAA";
    vector<int> ans;
    prefix_table(pattern, ans);
    int j = kmpsearch(text,pattern,ans);
    return 0;
}
```

