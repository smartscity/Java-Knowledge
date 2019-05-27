# Q&A

## 求一个数的平方根

* [牛顿-拉夫逊方法（Newton-Raphson method）](https://blog.csdn.net/xinm1001/article/details/52938113)
* 第二种方法

```text
public static BigDecimal sqrtRoot(BigDecimal m) {
        if ( m.compareTo( new BigDecimal(0)) == 0 ) {
            return new BigDecimal(0);
        }
​
        BigDecimal i = new BigDecimal("0");
        BigDecimal x1 = new BigDecimal("0");
        BigDecimal x2 = new BigDecimal("0");
        while (   ( i.multiply(i) ).compareTo(m)  == -1 ) {
            i =  i.add( new BigDecimal(0.1).setScale(8, RoundingMode.HALF_UP));
        }
​
        x1 = i;
        for ( int j = 0; j < 10; j++) {
            x2 = m;
            x2 = x2.divide(x1, 8, RoundingMode.HALF_UP);
            x2 = x2.add(x1);
            x2 = x2.divide(new BigDecimal("2"), 8, RoundingMode.HALF_UP);
            x1 = x2;
        }
        return x2;
    }
​
    for ( int i = 0; i <= 10; i++ ) {
        System.out.println(i + " : " + sqrtRoot( new BigDecimal(i)));
    }
```

* 参考文献
  * [https://blog.csdn.net/qq\_17776287/article/details/53762265](https://blog.csdn.net/qq_17776287/article/details/53762265) 



