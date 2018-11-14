# 堆外内存

### 前传

* 查看GC参数



  * `-verbose:gc -XX:HeapDumpPath=. -Xloggc:gc.log -XX:+PrintGC -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+UseGCLogFileRotation -XX:+DisableExplicitGC -XX:+PrintTenuringDistribution`

### 堆外内存

* Direct Memory
* 堆内存有JVM自己管理，而堆外内存只有 full gc触发时才会被GC，所以我们需要手动做适当的内存回收工作。



### 回收机制

* 受GC控制

### **DirectByteBuffer**

### **代码实例**

```java
public class NonHeapTest {
    public static void clean(final ByteBuffer byteBuffer) {  
        if (byteBuffer.isDirect()) {  
           ((DirectBuffer)byteBuffer).cleaner().clean();  
        }  
  }  
    
    public static void sleep(long i) {  
        try {  
              Thread.sleep(i);  
         }catch(Exception e) {  
              /*skip*/  
         }  
    }  
    public static void main(String []args) throws Exception {  
           ByteBuffer buffer = ByteBuffer.allocateDirect(1024 * 1024 * 200);  
           System.out.println("start");  
           sleep(5000);  
           clean(buffer);//执行垃圾回收
//         System.gc();//执行Full gc进行垃圾回收
           System.out.println("end");  
           sleep(5000);  
    }  
}
```

\*\*\*\*

