# 排序、kmp、队列、栈

## 怎么学

做笔记，将知识**转化吸收成自己**的

1. 整理并写博客，听懂到自己吸
2. 收会有很多损失，要转化和吸收。
3. 不懂要及时沟通，搞懂搞透。带有冲大厂和涨薪的目的学，写好简历找老师模拟面试
4. 面试中暴露的问题找老师复盘

## 排序

[参考](https://www.cnblogs.com/onepixel/articles/7674659.html)

注意：三种基本的算法都能引申出复杂的算法，插入、冒泡、选择。

1. 希尔排序

   shellsort是**插入排序**的推广，先对相距远的元素对排序，快速减少大量的无序状态，减少小距离的排序工作，避免插入排序的大偏移，再缩小比较的元素间的距离（最后为1则是**插入排序**），因而能比简单的相邻交换更快地将远处离位元素移动到它真正的位置上，因此运行时间取决于使用的间隔序列。

   ![image-20211016143251831](C:\Users\yl\AppData\Roaming\Typora\typora-user-images\image-20211016143251831.png)

   三点：分组次数、每次组循环、组内循环。

   ```cpp
   // Start with a big gap, then reduce the gap
   for (int gap = n/2; gap > 0; gap /= 2)//分组次数
   {
       // Do a gapped insertion sort for this gap size.
       // The first gap elements a[0..gap-1] are already in gapped order
       // keep adding one more element until the entire array is gap sorted
       for (int i = gap; i < n; i += 1)//每组遍历
       {
           // add a[i] to the elements that have been gap sorted
           // save a[i] in temp and make a hole at position i
           int temp = arr[i];
           // shift earlier gap-sorted elements up until the correct location for a[i] is found
           int j;           
           //组内排序
           for (j = i; j >= gap && arr[j - gap] > temp; j -= gap)arr[j] = arr[j - gap];
           //  put temp (the original a[i]) in its correct location
           arr[j] = temp;
       }
   }
   ```

2. 快速排序

   quicksort是**冒泡排序**的改进，类似**归并排序**，也是分治算法，选择一个元素作为“pivot”，并将小于其值的放左边，大于其值的放右边，对pivot左右两边的分区递归运行上述过程，每次递归，每调用一次，确定一个值的正确位置。左小右大，则左半部分不用和右半部分比较，大大减少比较次数。选pivot主要有：

   + 第一个元素

   + 最后一个元素

   + 随机元素

   + 中间元素

   ![image-20211016154712094](C:\Users\yl\AppData\Roaming\Typora\typora-user-images\image-20211016154712094.png)

   ```cpp
   void quick_sort(int a[],int l,int r){
       if(l>=r) return ;
       int i=l-1,j=r+1,temp=a[i+j>>1];
       
       while(i<j){
           do i++;while(a[i]<temp);
           do j--;while(a[j]>temp);
           if(i<j) swap(a[i],a[j]);
       }
       quick_sort(a,l,j);
       quick_sort(a,j+1,r);
   }
   ```

3. 归并排序

   mergesort是分治算法，速度仅次于快排，用空间换时间，复杂度最好最坏都为o(nlogn)，同时是稳定排序。包含三个部分：

   - **分解（Divide）**：将n个元素分成个含n/2个元素的子序列。
   - **解决（Conquer）**：用合并排序法对两个子序列递归的排序。
   - **合并（Combine）**：合并两个已排序的子序列已得到排序结果

   ![image-20211016160749009](C:\Users\yl\AppData\Roaming\Typora\typora-user-images\image-20211016160749009.png)

   ```cpp
   if(l>=r)return ;
   int mid = l + r >> 1;	
   //分解
   merge_sort(q, l, mid), merge_sort(q, mid + 1, r);
       
   int k=0,i=l,j=mid+1;
   //合并
   while(i<=mid&&j<=r)
   	if(q[i]<=q[j])temp[k++]=q[i++];
       else temp[k++]=q[j++];
   //剩余    
   while(i<=mid)temp[k++]=q[i++];
   while(j<=r)temp[k++]=q[j++];
       
   for(i=l,j=0;i<=r;i++,j++)q[i]=temp[j];
   ```

   

4. [堆排序]([AcWing 838. 堆排序 - AcWing](https://www.acwing.com/solution/content/29416/))

   heapsort是**选择排序**的改进，用堆维护未排序列表，以便更快的（而非**选择排序**的线性扫描）再每步中找到最值元素并插入有序列表。堆排序有两个关键操作：up和down，这里以小根堆为例。

   + up:插入时将值放数组末尾，然后不断向上调整
   + down:删除堆顶元素时将堆顶和数组尾元素交换，移除数组末尾节点，并向下调整直至满足堆性质

   注意：Remove某个下标节点时，和extract类似，但调整时既可能向上也可能向下，都需处理。

   ![image-20211016211332865](C:\Users\yl\AppData\Roaming\Typora\typora-user-images\heapSort.gif)

   ```cpp
   void down(int u)
   {
       int t=u;
       //判断是否左右孩子更小
       if(u*2<=sizenum&&h[u*2]<h[t])t=u*2;
       if(u*2+1<=sizenum&&h[u*2+1]<h[t])t=u*2+1;
       if(u!=t)
       {
           swap(h[u],h[t]);
           down(t);
       }
   }
   void up(int u)
   {
       while(u/2&&h[u/2]>h[u])
       {
           swap(h[u/2],h[u]);
           u/=2;
       }
   }
   ```

   简单总结

   ![image-20211016212113030](C:\Users\yl\AppData\Roaming\Typora\typora-user-images\image-20211016212113030.png)

5. [kmp](https://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)

   [参考2](https://www.acwing.com/solution/content/14666/)

   kmp是堆暴力搜索的优化，即若不匹配则把搜索位置往后移一位，因为把搜索位置移到已比较过的位置，重比一遍效率差，因而通过部分匹配表（“部分匹配值”：前缀（除最后元素外，一个字符串全部头部组合）和后缀（处开始元素外，一个字符串全部尾部组合）的最长共有元素的长度）优化计算出真正的移动位数（移动位数 = 已匹配的字符数 - 对应的部分匹配值）。面试中不会直接考，但涉及到字符匹配可用kmp（难点是next）

   ```cpp
   #include<iostream>
   using namespace std;
   const int N=100010,M=1000010;
   int n,m;
   char p[N],s[M];
   int ne[N];
   int main(){
       cin>>n>>p+1>>m>>s+1;
       
       for(int i=2,j=0;i<=n;i++){
           while(j && p[i]!=p[j+1]) j=ne[j];
           if(p[i]==p[j+1]) j++;
           ne[i]=j;
       }
       for(int i=1,j=0;i<=m;i++){
           while( j&& s[i] !=p[j+1]) j=ne[j];
           if(s[i]==p[j+1]) j++;
           if(j==n){
               cout<<i-n<<" ";
               j=ne[j];
           }
       }
       return 0;
   }
   ```

   



## 链表

1. 链表找倒数第n个节点
2. 判断链表是否有环
3. 判断两链表是否有公共节点
4. 链表反转

