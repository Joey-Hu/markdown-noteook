## JAVA泛型
Java泛型（generics）是JDK 5中引入的一个新特性，泛型提供了编译时类型安全监测机制，该机制允许程序员在编译时监测非法的类型。使用泛型机制编写的程序代码要比那些杂乱地使用Object变量，然后再进行强制类型转换的代码具有更好的安全性和可读性。泛型对于集合类尤其有用，例如，ArrayList就是一个无处不在的集合类。

泛型的本质是参数化类型，也就是所操作的数据类型被指定为一个参数。

泛型类型在逻辑上看以看成是多个不同的类型，实际上都是相同的基本类型。

#### 泛型的使用
泛型有三种使用方式：泛型类、泛型接口和泛型方法

**1. 泛型类**
通过泛型可以完成一组类的操作对外开放相同的接口。最典型的就是各种容器类（List, Set, Map）。

泛型类举例：
```java
// 泛型类定义：
// 此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
// 在实例化泛型类时，必须指定T的具体类型
public class Generic<T>{ 
    // key这个成员变量的类型为T,T的类型由外部指定  
    private T key;

    public Generic(T key) { // 泛型构造方法形参key的类型也为T，T的类型由外部指定
        this.key = key;
    }

    public T getKey(){ // 泛型方法getKey的返回值类型为T，T的类型由外部指定
        return key;
    }
}

// 泛型类实例化
//泛型的类型参数只能是类类型（包括自定义类），不能是简单类型
//传入的实参类型需与泛型的类型参数类型相同，即为Integer.
Generic<Integer> genericInteger = new Generic<Integer>(123456);

//传入的实参类型需与泛型的类型参数类型相同，即为String.
Generic<String> genericString = new Generic<String>("key_vlaue");
Log.d("泛型测试","key is " + genericInteger.getKey());
Log.d("泛型测试","key is " + genericString.getKey());
```
定义的泛型类，就一定要传入泛型类型实参么？并不是这样，在使用泛型的时候如果传入泛型实参，则会根据传入的泛型实参做相应的限制，此时泛型才会起到本应起到的限制作用。如果不传入泛型类型实参的话，在泛型类中使用泛型的方法或成员变量定义的类型可以为任何的类型。

> 注意：
> 1. 泛型的类型参数只能是类类型（Boolean, Byte, Char, Short, Integer, Float, Double ），不能是简单类型。

**2. 泛型接口**
**3. 泛型方法**

参考：
https://www.jianshu.com/p/41a7a975502d
https://blog.csdn.net/s10461/article/details/53941091