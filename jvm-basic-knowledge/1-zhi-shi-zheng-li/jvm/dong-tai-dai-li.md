# 动态代理

## JVM 动态代理

* **JVM 动态代理**
  * 不支持集成，因为默认集成了 proxy
* **CGLib 代理**
  * 全支持
  * spring 默认使用 JVM动态代理，当无法使用时，使用Cglib

## 上代码

```text
public interface HelloWorld {
    void sayHello(String name);
}
```

```text
public class HelloWorldImpl implements HelloWorld {
    @Override
    public void sayHello(String name) {
        System.out.println("Hello " + name);
    }
}
```

```text
public class CustomInvocationHandler implements InvocationHandler {
    private Object target;
​
    public CustomInvocationHandler(Object target) {
        this.target = target;
    }
​
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before invocation");
        Object retVal = method.invoke(target, args);
        System.out.println("After invocation");
        return retVal;
    }
}
```

```text
public class ProxyTest {
​
    public static void main(String[] args) throws Exception {
        System.getProperties().put("sun.misc.ProxyGenerator.saveGeneratedFiles", "true");
​
        CustomInvocationHandler handler = new CustomInvocationHandler(new HelloWorldImpl());
        HelloWorld proxy = (HelloWorld) Proxy.newProxyInstance(
                ProxyTest.class.getClassLoader(),
                new Class[]{HelloWorld.class},
                handler);
        proxy.sayHello("Mikan");
    }
​
}
```

```text
        // 获取计数  
        long num = nextUniqueNumber.getAndIncrement();  
        // 默认情况下，代理类的完全限定名为：com.sun.proxy.$Proxy0，com.sun.proxy.$Proxy1……依次递增  
        String proxyName = proxyPkg + proxyClassNamePrefix + num;  

        // 这里才是真正的生成代理类的字节码的地方  
        byte[] proxyClassFile = ProxyGenerator.generateProxyClass(  
            proxyName, interfaces); 
​
public final class $Proxy0 extends Proxy implements HelloWorld {
```

* [http://blog.csdn.net/mhmyqn/article/details/48474815](http://blog.csdn.net/mhmyqn/article/details/48474815)
* [http://blog.csdn.net/u013126379/article/details/52121096](http://blog.csdn.net/u013126379/article/details/52121096)



