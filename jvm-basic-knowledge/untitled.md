# Q&A

### 1、JVM OOM问题查找过程

### 2、第一次遇到JVM堆外内存溢出BUG查找

### 3、[How to Check if an Array Contains a Value in Java Efficiently?](https://www.programcreek.com/2014/04/check-if-array-contains-a-value-java/)

```text
public static boolean useArraysBinarySearch(String[] arr, String targetValue) { 
    int a =  Arrays.binarySearch(arr, targetValue);
    if(a > 0)
        return true;
    else
        return false;
}
```

### 4、**A little Quiz: Why substring\(\) method in JDK 6 can cause memory leaks?**

* 同下

### 5、[The substring\(\) Method in JDK 6 and JDK 7](https://www.programcreek.com/2013/09/the-substring-method-in-jdk-6-and-jdk-7/)

* 存在性能问题
* 1.7做了优化

