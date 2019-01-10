# 时间复杂度

### 图示

![](../.gitbook/assets/image%20%2840%29.png)

### 公式

| 顺序排序 |  |  |
| :--- | :--- | :--- |
| O\(1\) | 常数阶 |  |
| O\(log2n\) / O\(lgn\) | 对数阶 | count = count \* 2 |
| O\(n\) | 线性阶 |  |
| O\(nlog2n\) | 线性对数 |  |
| O\(n^2\) | 平方阶 |  |
| O\(n^3\) | 立方阶 |  |
| O\(n^k\) | K次方阶 |  |
| O\(2^n\) | 指数阶 |  |

### 详细介绍

* 常数阶`O(1)`

  ```text
  k次方阶O(n^k),指数阶O(2^n)
  ```

* 线性阶`O(n)`

  ```text
  for(int i = 0; i < n; i++){
      printf("%d ",i);
  }  
  ```

* 平方阶`O(n^2)`

  ```text
  for(int i = 0; i < n; i++){
      for(int j = 0; j < n; j++){
          printf("%d ",i);
      }
  }   
  //运行次数为(1+n)*n/2
  //时间复杂度O(n^2)
  ```

* 对数阶`O(log2n)`

  ```text
  int i = 1, n = 100;
  while(i < n){
      i = i * 2;
  }
  //设执行次数为x. 2^x = n 即x = log2n
  //时间复杂度O(log2n)
  ```

* 线性对数阶`O(nlog2n)`
* 立方阶`O(n^3)`
* k次方阶`O(n^k)`
* 指数阶`O(2^n)`

### **常用排序算法的时间复杂度**

```text
      最差时间分析  平均时间复杂度 稳定度     空间复杂度   
冒泡排序    O(n2)   O(n2)       稳定        O(1)  
快速排序    O(n2)   O(n*log2n)  不稳定  O(log2n)~O(n)     
选择排序    O(n2)   O(n2)       稳定      O(1)    
二叉树排序  O(n2) O(n*log2n)    不稳定     O(n)     
插入排序    O(n2)   O(n2)       稳定      O(1)    
堆排序  O(n*log2n) O(n*log2n)   不稳定     O(1)    
希尔排序    O        O          不稳定     O(1)
```

### **ArrayList vs LinkedList**

```text
                   | Arraylist | LinkedList
 ------------------------------------------
 get(index)        |    O(1)   |   O(n)
 add(E)            |    O(n)   |   O(1)
 add(E, index)     |    O(n)   |   O(n)
 remove(index)     |    O(n)   |   O(n)
 Iterator.remove() |    O(n)   |   O(1)
 Iterator.add(E)   |    O(n)   |   O(1)
```



### 参考文献

* [https://www.jianshu.com/p/f4cca5ce055a](https://www.jianshu.com/p/f4cca5ce055a)

