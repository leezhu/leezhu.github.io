# 快速排序法两种代码实现方式讲解

<!--more-->
快速排序法的原理我就不多讲了，就是基于分冶的思想，所以一般用递归调用实现。至于具体原理网上一搜一大把，而我要讲的就是代码实现中具体代码部分的讲解。

## 第一种实现方式
```c++
//快速排序第一种方式实现
void quick_sort(int s[], int l, int r)  
{  
    if (l < r)  
    {  
        //Swap(s[l], s[(l + r) / 2]); //将中间的这个数和第一个数交换 参见注1  
        int i = l, j = r, x = s[l];  
        while (i < j)  
        {  
            while(i < j && s[j] >= x) // 从右向左找第一个小于x的数  
                j--;    
            if(i < j)   
                s[i++] = s[j];  

            while(i < j && s[i] < x) // 从左向右找第一个大于等于x的数  
                i++;    
            if(i < j)   
                s[j--] = s[i];  
        }  
        s[i] = x;  
        quick_sort(s, l, i - 1); // 递归调用   
        quick_sort(s, i + 1, r);  
    }  
} 
```
其实这种方法，在网上是讲的最多的，而且很详细，就是从两端开始。i,j分别与比较量(一般是首个数或者中间量)进行比较，比它小的替换掉放一边，比它大的放另外一边。最后再把比较量放到分割位置。

## 第二种实现方式
```c++
//快速排序第二种实现方式
void qsort(int v[],int left,int right)
{
    int i, last;
    void swap(int v[],int i,int j);//交换两个数

    if(left >= right)
        return;
    swap(v,left,(left+right)/2);//将中间数交换，防止第一个数就是最小或者最大，减小复杂度
    last=left;//第一个数为比较量
    for(i=left+1;i<=right;i++)
        if(v[i]<v[left])
            swap(v,++last,i);

    swap(v,left,last);
    qsort(v,left,last-1);
    qsort(v,last+1,right);
}

```
可以发现，这种代码实现方式比第一种简洁的很多，但是不够直观.

**关键代码如下**

```c++
    last=left;//第一个数为比较量
    for(i=left+1;i<=right;i++)
        if(v[i]<v[left])
            swap(v,++last,i);

    swap(v,left,last);
```

第二种方式的实现是从一端开始的，我们可以这么想，比较量是第一个，即v[left]。后面的数v[i]都和它进行比较，i是个移动量，而last是一个在数组中做标记的，在标记的数后面的数都是比v[left]小的数，
每当v[i]小于v[left],last应该先+1,因为它自身是满足小于v[left]，所以需要将前面不满足的数替换掉，然后自己又指向了这个新的数，i也往后移一位。最后再将这个分割量v[left]交换到分割位置last。

这是对第二种实现方式代码[图解方式](http://www.cnblogs.com/kaituorensheng/archive/2013/02/23/2923771.html)

    原文出自我的csdb[博客](https://blog.csdn.net/zlhn55/article/details/49155001)
>许可协议：[CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


