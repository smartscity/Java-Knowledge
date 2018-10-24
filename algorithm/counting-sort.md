---
description: 计数排序算法
---

# Counting Sort

#### 基本属性 <a id="ji-ben-shu-xing"></a>

* **空间复杂度**
  * O\(MAX-MIN\) 用空间换时间
* **时间复杂度**
  * O\(n\)
* **适用范围**
  * 待排序数组值域较窄，而待排序元素个数较多时，比较节省空间，反之则非常浪费空间
  * 适用所有数字排序 int32 int64 \[0, 2^32\] ~ \[0, 2^64\] 之间
* **关键步骤**
  * **Step 1:** 遍历待排序数据array\[N\]，在计数器数组\(count\[MAX-MIN\]\)中记录出现次数； **e.g.**`count[array[index]] ++`
  * **Step 2:** 遍历count\[MAX-MIN\]，得到 array\[N\]，排序结束
* **代码**

{% code-tabs %}
{% code-tabs-item title="CountingSort" %}
```java
public static void main(String[] args) {
    int[] array = new int[]{5, 7, 1, 8, 9, 2, 3, 4, 1, 3, 6, 2, 4, 0};
    int[] count = new int[9 - 0 + 1];
    // counting sort
    for (int i = 0; i < array.length; i++) {
        count[array[i]] = count[array[i]] + 1;
    }
    // iterate counting
    for (int i=0;i<count.length;i++){
        int temp = count[i];
        for(int j=0;j<temp;j++){
            System.out.print(i);
        }
    }
    // result
    // 01122334456789
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* **图示**

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LPU-bbQ7pwQnDAbfs6g%2F-LPV2TwfoPmYqpNXTDlM%2F-LPV8yA-25qFOcEx5vnT%2Fimage.png?alt=media&token=6a4446f1-5d2e-453a-82db-e511d0143dda)

![](../.gitbook/assets/image%20%287%29.png)



