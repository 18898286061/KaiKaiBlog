# JS数组去重的几种常见方法


 ## 一、简单的去重方法

 下面我们先介绍以下 **Array.indexOf(item,start)**:
 
 indexOf() 方法可返回数组中某个指定的元素位置。

 该方法将从头到尾地检索数组，看它是否含有对应的元素。开始检索的位置在数组 start 处或数组的开头（没有指定 start 参数时）。如果找到一个 item，则返回 item 的第一次出现的位置。开始位置的索引为 0。

 如果在数组中没找到指定元素则返回 -1。

 提示如果你想查找字符串最后出现的位置，请使用 lastIndexOf() 方法。

```
 // 最简单的数组去重法
 
 // 新建一新数组，遍历传入数组，值3不在新数组就push进该新数组中
 // 注： IE8以下不支持数组的indexOf方法
 
 function uniq(array){
  var temp = []; // 一个新的临时数组
  for(var i = 0; i < array.length; i++) {
   if(temp.indexOf(array[i] == -1)){
    temp.push(array[i]);
   }
  }
  return temp;
 }
 
 var aa = [1,2,3,4,5,4];
 console.log(uniq(aa))
 
```

## 二、利用对象键值对中键的唯一性实现数组去重

```
var arr = [1,2,3,2,3,4,5,6,7,8,9,8,5]; 
			
			//将数组转换成对象
			 //利用对象的key值不能重复这一特性
			var toObject = function(array) {
			  var obj = {};
			  for(var i=0,j=array.length;i<j;i++){
			  	obj[array[i]] = true;
			  }
			  return obj;
			}
			
			//再将对象转换成数组
			var toArray = function(obj){
				
				var arr = [];
				for(var i in obj){
					arr.push(i);
				}
				return arr;
			}
			
			//综合前两者，完成去数组重复项方法
			var uiq = function(arr){
				return toArray(toObject(arr));
			}
			var arr1 = uiq(arr);
			console.log(arr1); 

```




## 三、排序后相邻去除法
```
/*
* 给传入数组排序，排序后相同值相邻，
* 然后遍历时,新数组只加入不与前一值重复的值。
* 会打乱原来数组的顺序
* */
function uniq(array){
    array.sort();
    var temp=[array[0]];
    for(var i = 1; i < array.length; i++){
        if( array[i] !== temp[temp.length-1]){
            temp.push(array[i]);
        }
    }
    return temp;
}

var aa = [1,2,"2",4,9,"a","a",2,3,5,6,5];
console.log(uniq(aa));
```

## 四、数组下标法

```
/*
*
* 还是得调用“indexOf”性能跟方法1差不多，
* 实现思路：如果当前数组的第i项在当前数组中第一次出现的位置不是i，
* 那么表示第i项是重复的，忽略掉。否则存入结果数组。
* */
function uniq(array){
    var temp = [];
    for(var i = 0; i < array.length; i++) {
        //如果当前数组的第i项在当前数组中第一次出现的位置是i，才存入数组；否则代表是重复的
        if(array.indexOf(array[i]) == i){
            temp.push(array[i])
        }
    }
    return temp;
}

var aa = [1,2,"2",4,9,"a","a",2,3,5,6,5];
console.log(uniq(aa));
```



## 五、优化遍历数组法
```
// 思路：获取没重复的最右一值放入新数组
/*
* 推荐的方法
*
* 方法的实现代码相当酷炫，
* 实现思路：获取没重复的最右一值放入新数组。
* （检测到有重复值时终止当前循环同时进入顶层循环的下一轮判断）*/
function uniq(array){
    var temp = [];
    var index = [];
    var l = array.length;
    for(var i = 0; i < l; i++) {
        for(var j = i + 1; j < l; j++){
            if (array[i] === array[j]){
                i++;
                j = i;
            }
        }
        temp.push(array[i]);
        index.push(i);
    }
    console.log(index);
    return temp;
}

var aa = [1,2,2,3,5,3,6,5];
console.log(uniq(aa));
```




## 六、Set 去重
```
 Array.from(new Set(a))
```
