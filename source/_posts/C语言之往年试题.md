---
title: C语言之往年试题
date: 2020-03-12 10:43:18
tags: C语言
categories: 复试
---
## 数组指针和指针数组
![sd](/C语言之往年试题/1.png)
```bash
首先判断上面那个是一个数组，然后是个指针类型的，所以是指针数组
下面的首先是个指针，然后是个数组，所以是一个指向int数组的数组指针
```
![sd](/C语言之往年试题/2.png)
```bash
一个指针相加一个常数，使用这个指针所指向的类型的所占内存大小乘以常数然后相加
```
 <!--more--> 
## 排序算法
### 冒泡排序
```bash
  for(i=0;i<n&&find;i++)
    {
        find=0;
        for(j=n-1;j>i;j--)//这里的结束条件在i后面一位，所以下面的判断语句是要减去1
        {
            if(a[j-1]> a[j])
            {
                find=1;
                int temp =a[j-1];
                a[j-1] =a[j];
                a[j] =temp;
            }
        }
    }
   for(i=0;i<n&&find;i++)
    {
        find=0;
        for(j=0;j<n-1-i;j++)//这里的结束条件是在i的前面一位，所以判断语句要加上1
        {
            if(a[j]>a[j+1])
            {
                find=1;
            }
        }
    }
```
### 选择排序
```bash
 for (i=0; i<n-1; i++)//每次把最小的放在某一个位置上
    {
        for (j=i+1; j<n; j++)
        {
            if (strcmp(ptr[j], ptr[i])< 0)
            {
                temp = ptr[i];
                ptr[i] = ptr[j];
                ptr[j] = temp;
            }
        }
    }
 for(i=0;i<n;i++)
    {
        int min=i;//每次的这个位置假设是最小的
        for(j =i+1;j<n;j++)
        {
            if(a[j]<a[min])
                min=j;
        }
        if(min!=i)//相比于上面那个优化的步骤在这可能不用交换
        {
            int temp =a[i];
            a[i]=a[min];
            a[min]=temp;
        }
    }

```
### 归并排序
```bash
void Merge(int a[], int b[], int c[], int m, int n)
{
    int i=0,j=0;
    int index=0;
    while(i<m && j <n)
    {
        if(a[i] >b[j])
        {
            c[index++] =a[i++];
        }
        else
        {
            c[index++] =b[j++];
        }
    }
    while(i<m)
    {
        c[index++] =a[i++];
    }
    while(j<n)
    {
        c[index++] =b[j++];
    }
}
```
### 插入排序
```bash
for(i=n-1;i>=0;i--)//这一题是单次插入
    {
        if(a[i]>x)
        {
            a[i+1]=a[i];
        }
        else
        {
            a[i+1]=x;//这里在比较的过程中减去了1所以应该再加回来
            break;
        }
    }
void InsertSort(int a[],int n)
{
    int i ,j,temp;
    for(i=1;i<n;i++)
    {
        temp = a[i];//从第二个数开始向前面的数组插入
        j=i-1;
        while(j>=0 &&a[j]>temp)
        {
            a[j+1] =a[j];
            j--;
        }
        a[j+1]=temp;//这里因为比较的过程中向前移动了以为所以要加个一
    }
}
```
### 快速排序
```bash
int Partition(int a[],int low,int high)//一次划分可以把所有小于标准值的数放在左边，所有大于标准值的数放在右边
{
    int pivot= a[low];
    while(low<high)
    {
        while(low<high&& a[high]>=pivot)
            high--;
        swap(&a[low],&a[high]);
        while(low<high && a[low]<=pivot)
            low++;
        swap(&a[low],&a[high]);

    }
    a[low]=pivot;
    return low;
}
void QuikSort(int a[],int low,int high)//递归实现多次划分
{
    if(low<high)
    {
        int temp = Partition(a,low,high);
        QuikSort(a,low,temp-1);
        QuikSort(a,temp+1,high);
    }
}
```
### 堆排序
```bash
void HeapAdjust12(int a[],int begin,int end)
{
    int temp=a[begin];
    int i ;
    for(i=2*begin;i<=end;i=i*2)
    {
        if(a[i]<a[i+1] &&i<end)
        {
            i++;
        }
        if(temp>a[i])//这里确定了是采用大根堆的形式，因为如果比较大就直接跳过了
            break;
        a[begin] =a[i];
        begin = i;
    }
    a[begin] = temp;
}
void HeapSort2(int a[],int n )
{
    int i;
    for(i=n/2;i>0;i--)
    {
        HeapAdjust12(a,i,n);//初始化为大顶堆
    }
    for(i=n;i>0;i--)
    {
        int temp=a[1];
        a[1]=a[i];
        a[i]=temp;//将最大的元素放在最后一个 交换下来
        HeapAdjust12(a,1,i-1);
    }
}
```
## 往年试题
### 大数阶乘问题
思想：关键是要设置一个进位变量carry
```bash
方法一：
for(i=0;i<n;i++)
    {
        for(j=0;j<index;j++)
        {
            int temp =(a[j]*(i+1)+carry);//这里的相乘一定是i+1，因为这里存储的下标是从0
            开始的，同样打印语句中答应的下标要从index-1开始
            a[j]= temp%10 ;
            carry = temp /10;
        }
        while(carry)
        {
            a[index] = carry%10;
            carry=carry/10;
            index++;
        }
    }
程序改错方法二：
for (i = 1; i <= m; i++)
    {
        for (j = 1; j <= index; j++)
        {
            data[j] = data[j] * i;
        }//其实思想是一样的这里是全部先乘完之后然后再去做进位运算
        for (k = 1; k < index; k++)
        {
            if (data[k] >= 10)
            {
                data[k + 1] = data[k + 1] + data[k] / 10;
                data[k] = data[k] % 10;
            }
        }
        while (data[index] >= 10 && index <= SIZE - 1)
        {
            data[index+1] = data[index] / 10 + data[index+1];
            data[index] = data[index] %10;
            index ++;
        }
    }
```
### 试题：将句子拆成单词
```bash
int i=0,j=0,k=0;
    while(i<len)
    {
        k=0;//每次都要初始化，然后赋值给下一个字符串
        while(a[i]!=' ' &&i<len)
        {
            ss[j][k]=a[i];
            k++;
            i++;
        }
        ss[j][k] ='\0';
        j++;
        i++;
    }
```
