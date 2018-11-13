# 字节码

### 字节码 Code 示例：

```java
public int greaterThen(int intOne, int intTwo) {
    if (intOne > intTwo) {
        return 0;
    } else {
        return 1;
    }
}
```

#### 执行 `javap` 之后，得到字节码如下：

```java
0: iload_1
1: iload_2
2: if_icmple     7
5: iconst_0
6: ireturn
7: iconst_1
8: ireturn
```

#### 字节码含义如下：

| 字节码 | 含义 |
| :--- | :--- |
| iconst\_0 | int i=0 |
| inc | i++ |
| if\_icmp`<cond>` `eq ne lt le gt ge` | if \[g = &lt;,l = &gt;\] |
| bipush | byte as int push into Operand Stack; `inti = 5`;`to`0: bipush 5\` |
| istore\_0 | 压栈 至 索引`0`处 |
| aload\_0 | 取栈 第 `0` 即栈顶 |
| invokespecial | 调用Object 的 init 构造方法 |
| putfield |  |
| putstatic |  |
| ireturn | 返回 int |
|  |  |

