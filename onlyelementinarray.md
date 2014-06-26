##找出数组中唯一的重复元素

- 1-1000放在含有1001个元素的数组中，只有唯一的一个元素值重复，其它均只出现 一次。每个数组元素只能访问
一次，设计一个算法，将它找出来；不用辅助存储空间，能否设计一个算法实现？

- Method1:将1001个元素相加减去1,2,3,……1000数列的和，得到的差即为重复的元素。

```
int Find(int *a)   
{   
    int i;//变量   
    for(i = 0; i<=1000 ;i++)   
    {   
       a[1000] += a[i];   
    }   
    a[1000] -= (i*(i-1))/2       //i的值为1001   
    return a[1000];   
}
```

- Method2: 利用下标与单元中所存储的内容之间的特殊关系，进行遍历访问单元，一旦访问过的单元赋予一个标记
，利用标记作为发现重复数字的关键。代码如下：


```
void findrepeat(int array[], int length)
{
    int index=array[length-1]-1;
    while ( true )
    {
        if ( array[index]<0 )
            break;
        array[index]*=-1;
        index=array[index]*(-1)-1;
    }
   cout<<"The repeat number is "<<index+1<<endl;
}
```

- Think：index是array的指示器，根据代码可以知道index是array相应的值-1，因为只有一个数重复，index只可能
进入每个array[i]一次，如果进入两次则必是重复，index+1,即为重复的值。

- [http://veryti.com/question/400](http://veryti.com/question/400)---原文地址
