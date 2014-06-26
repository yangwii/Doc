##找出数组中唯一的重复元素

- 1-1000放在含有1001个元素的数组中，只有唯一的一个元素值重复，其它均只出现 一次。每个数组元素只能访问
一次，设计一个算法，将它找出来；不用辅助存储空间，能否设计一个算法实现？

```
void ipeat(int array[], int length)
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
