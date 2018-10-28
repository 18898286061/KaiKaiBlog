[本文转至：前端面试必备——基本排序算法 作者：owen1190](https://blog.csdn.net/owen1190/article/details/76215932)


# 使用JS实现的排序算法

## 插入排序

![Alt text](https://user-gold-cdn.xitu.io/2017/7/27/da44baba996d9c4e8ddeb43a01c2139d)

算法描述： 

1. 从第一个元素开始，该元素可以认为已经被排序 
2. 取出下一个元素，在已经排序的元素序列中从后向前扫描 
3. 如果该元素（已排序）大于新元素，将该元素移到下一位置 
4. 重复步骤 3，直到找到已排序的元素小于或者等于新元素的位置 
5. 将新元素插入到该位置后 
6. 重复步骤 2~5

现有一组数组 arr = [5, 6, 3, 1, 8, 7, 2, 4]

```
[5] 6 3 1 8 7 2 4  //第一个元素被认为已经被排序

[5, 6]  3 1 8 7 2 4 //6与5比较，放在5的右边

[3, 5, 6]  1 8 7 2 4 //3与6和5比较，都小，则放入数组头部

[1, 3, 5, 6]   8 7 2 4 //1与3,5,6比较，则放入头部

[1,3, 5, 6, 8]   7 2 4

[1, 3, 5, 6, 7, 8]  2 4

[1, 2, 3, 5, 6, 7, 8] 4

[1, 2, 3, 4，5, 6, 7, 8] 
--------------------- 

编程思路： 双层循环，外循环控制末排序的元素，内循环控制已排序的元素，将未排序的元素设为标杆，
与已排序的元素进行比较，小于则交换位置，大于则位置不动

```
function insertSort(arr) {
  var tmp;
  for(var i = 1; i < arr.length; i++) {
    for(var j = i; j >= 0; j--) {
      tmp = arr[i];
      if(arr[j - 1] > tmp) {
        arr[j] = arr[j - 1];
      } else {
        arr[j] = tmp;
        break;
      }
    }
  }
  return arr
}
  
```
时间复杂度O(n^2)


## 选择排序

![Alt text](https://user-gold-cdn.xitu.io/2017/7/27/e0824efdb79268d4de42991274dcc9eb)


算法描述：直接从待排序数组中选择一个最小（或最大）数字，放入新数组中。

```
[1] 5 6 3  8 7 2 4 
[1,2] 5 6 3  8 7  4 
[1,2,3] 5 6  8 7 2 4 
[1,2,3,4] 5 6 8 7
[1,2,3,4,5] 6  8 7 
[1,2,3,4,5,6] 8 7  
[1,2,3,4,5,6,7] 8  
[1,2,3,4,5,6,7,8] 
```

编程思路：先假设第一个元素为最小的，然后通过循环找出最小的元素，

```
  function selectSort(array)　{
    var length = array.length,
        i,
        j,
        minIndex,
        minValue,
        temp;
    for (i = 0; i < length - 1; i++) {
      minIndex = i;
      minValue = array[minInde];
      for (j = i + 1; j < length; j++) {
        if(array[j] < minValue) {
          minIndex = j;
          minValue =array[minIndex];
        }
      }
      // 交换位置
      temp = array[i];
      array[i] = minValue;
      array[minIndex] = temp;
    }
    return array
  }
```
时间复杂度O(n^2)










