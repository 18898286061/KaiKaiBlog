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

```

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


## 归并排序

![Alt text](https://user-gold-cdn.xitu.io/2017/7/27/f9fcaf0d64dcd11b9309f09062863b29)

算法描述：
1. 把n个记录看成n个长度为l的有序子表
2. 进行两两归并使记录关键字有序，得到 n/2 个长度为2的有序子表
3. 重复第2步直到所有记录归并成一个长度为n的有序表为止。

```
5 6 3 1 8 7 2 4

[5,6] [3,1] [8,7] [2,4]

[5,6] [1,3] [7,8] [2,4]

[5,6,1,3] [7,8,2,4]

[1,3,5,6] [2,4,7,8]

[1,2,3,4,5,6,7,8]
```


编程思路：将数组一直等分，然后合并

```
function merge(left, right) {
  var tmp = [];
  
  while (left.length && right.length) {
    if (left[0] < right[0]) {
      tmp.push(left.shift());
    } else {
      tmp.push(right.shift());
    }
  }
  return tmp.concat(left, right);
}

function mergeSort(a) {
  if (a.length === 1)
  return a;
}

var mid = Math.floor(a.length / 2),
    left = a.sclice(0, mid),
    right = a.slice(mid);
    
    return merge(mergeSort(left), mergeSort(right));
```
时间复杂度O(nlogn)

## 快速排序

![Alt text](https://user-gold-cdn.xitu.io/2017/7/27/ad4d6e25b6e0e91c743ae220e3d52d1e)

```
5 6 3 1 8 7 2 4

pivot
|
5 6 3 1 9 7 2 4
|
storeIndex

5 6 3 1 9 7 2 4//将5同6比较，大于则不更换
|
storeIndex

3 6 5 1 9 7 2 4//将5同3比较，小于则更换
  |
  storeIndex

3 6 1 5 9 7 2 4//将5同1比较，小于则不更换
    |
   storeIndex
...

3 6 1 4 9 7 2 5//将5同4比较，小于则更换
      |
      storeIndex

3 6 1 4 5 7 2 9//将标准元素放到正确位置
      |
storeIndex pivot
```

上述讲解了分区的过程，然后就是对每个子区进行同样做法

```
  fnction quickSort(arr) {
    if(arr.length <= 1) return arr;
    var partitionIndex = Math.floor(arr.length/2);
    var tmp = arr[partitionIndex];
    var left = [];
    var right = [];
    for (var i = 0; i < arr.length; i++) {
      if (arr[i] < tmp) {
        left.push(arr[i])
      } else {
        right.push(arr[i])
      }
    }
    return quickSort(left).concat([tmp], quickSort(right))
  }
```
上述版本会造成堆栈溢出，所以建议使用下面版本

原地分区版：主要区别在于先进行分区处理，将数组分为左小右大;

```
function quickSort(arr){
    function swap(arr,right,left){
        var tmp = arr[right];
        arr[right]=arr[left];
        arr[left]=tmp;
    }
    function partition(arr,left,right){//分区操作，
        var pivotValue=arr[right]//最右面设为标准
        var storeIndex=left;
        for(var i=left;i<right;i++){
            if(arr[i]<=pivotValue){
                swap(arr,storeIndex,i);
                storeIndex++;
            }
        }
        swap(arr,right,storeIndex);
        return storeIndex//返回标杆元素的索引值
    }
    function sort(arr,left,right){
        if(left>right) return;
        var storeIndex=partition(arr,left,right);
        sort(arr,left,storeIndex-1);
        sort(arr,storeIndex+1,right);
    }
    sort(arr,0,arr.length-1);
    return arr;
}
```


![Alt text](https://user-gold-cdn.xitu.io/2017/7/27/3bcdc49661b5c8a3500463095ecc09df)

算法描述： 
1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。 
2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。 
3. 针对所有的元素重复以上的步骤，除了最后一个。 
4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。


```
5 6 3 1 8 7 2 4

[5 6] 3 1 8 7 2 4 //比较5和6

5 [6 3] 1 8 7 2 4

5 3 [6 1] 8 7 2 4

5 3 1 [6 8] 7 2 4

5 3 1 6 [8 7] 2 4

5 3 1 6 7 [8 2] 4

5 3 1 6 7 2 [8 4]

5 3 1 6 7 2 4 8  // 这样最后一个元素已经在正确位置，所以下一次开始时候就不需要再比较最后一个

```

思路：外循环控制需要比较的元素，比如第一次排序后，最后一个元素就不需要比较了，内循环则负责两两元素比较，将元素放到正确位置上

```
function bubbleSort(arr){
    var len=arr.length;
    for(var i=len-1;i>0;i--){
        for(var j=0;j<i;j++){
            if(arr[j]>arr[j+1]){
                var tmp = arr[j];
                arr[j]=arr[j+1];
                arr[j+1]=tmp
            }
        }
    }
    return arr;
}
```
复杂度O(n^2)



