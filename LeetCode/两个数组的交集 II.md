# 两个数组的交集 II(Intersection of Two Arrays II)

利用Hash，因为Hash结构里面是不能有重复的元素。

新建一个`result`来保存两个数组重叠的交集，

  1. 首先判断下谁的数组长度比较短，短的那一个为Hash对象的复制体(如果两个数组的元素数目相差很多的话，优势就会体现出来，额外的空间就很少很多。)
  2. Hash对象复制完之后，接下来的for循环就是遍历另一个数组。与map中的元素进行比较，判断相同的话，把元素从map中删除，同时把该元素放入我们的res数组中。
  3. 在对map相减的时候，对map的长度进行判断，如果为0，就提前结束循环(刚好他们重复的元素都在数组的前半部分呢？这样后面就可以不再遍历了，特殊情况下还是可以减少时间的)

```
var intersect = function(nums1, nums2) {
    let resultArr = [];
    let hash = {};
    if(nums1.length < nums2.legth) {
        for(let i of nums1) {
            hash[i] = hash[i] + 1 || 1;
        }
        for(let i of nums2) {
            if(hash[i]) {
                resultArr.push(i)
                hash[i]--
                if(hash.size == 0) {
                    break;
                }
            }
        }
    } else {
        for(let i of nums2) {
            hash[i] = hash[i] + 1 || 1;
        }
        for(let i of nums1) {
            if(hash[i]) {
                resultArr.push(i)
                hash[i]--
                if(hash.size == 0) {
                    break;
                }
            }
        }
    }
    return resultArr;
};
```
