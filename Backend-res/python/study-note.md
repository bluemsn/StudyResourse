python 学习笔记

###基础篇 

开始学习 python  
python 是什么 不解释 狗哥去~
我是一名前端开发者 与之对比的都是js

开始：
python 的序列 和元组 
序列  对比javascript  的数组与json 的组合体 为什么呢 
因为与js相似但又 比js的数组强大了。

#####show 几个地方  
1. 
 写法上 `[]` 开始  `,` 分隔  与js 一致 
 可以嵌套 
	
	 > python  -->  arr = [['a','b','c'],'d'] 
	 > 
	 > js -->  var  arr = [['a','b','c'],'d'];


  	标点不一样    无var 整体感觉一致

2. 数组的length  与python的

   ````javascript
		var a=['a','b','c'];
		a.length;
		
   ````
   ````python
		a =['a','b','c'];
		len(a)		
   ````

3. 方法上 python 主要把方法集中在[]的参数中 并且可以赋值 来实现  js相似的功能 获取序列的长度  删除序列  增加序列 等等的方法
 1. 1
 2. 2
 3. 3
 4. 往序列末尾 追加元素 
  ````python
		 

  `````


4. 另 python 取整 是int (10.2)   --> 10 



5.  ` append  方法 `  用于在列表 末尾 添加完素

		>>> lst = [1,3,5]
  		>>> lst.append(4)  // ? 只可以加一个？
		>>> lst
		[1,3,5,4]  


6.   `entend` 

		>>> a = [1,3,4]
		>>> b = [3,4,5]
		>>> a.extend(b)
		>>> a
		[1,3,4,3,4,5]
		>>> a + b  //返回一个 全新 相加的列表  原列表不变化
		[1,3,4,3,4,5,3,4,5]
		>>> a 
		[1,3,4,3,4,5]

7.  ` insert `  比列表  lst[3:3]=[1,2,4] 直观  

		

8.   ` pop `  返回删除的列表项的值 是唯一个可返回列表项的方法

9.   ` remove `  删除列表项 无返回值  
10.   `reverse`  列表返序
11.   ` sort `  对原始列表进行排序 无返回值    
12.   `sorted`  不对原始列表进行排序  返回排序后的列表 也可以用于其它序列 对字符串 序列 操作 等   



   





