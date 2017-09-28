# java泛型

- **什么是泛型** ?

  我们来看一段代码:

``` java
public void static main(String args[])
{
    List list=new Arraylist();
  	list.add("String");
    list.add(100);
  
}
public void static out(List list){
    for(int i=0;i<list.size();i++)
    {
        String var=(String)list.get(i);  //出错 100是正数不是String
      	System.out.prinln("var :"+var);
    }
}
```

  编译通过，运行时出错，因为编译时候List 默认是Object  类型所以不会出错，但是运行时类型强制转换就会出错，那么有没有一种方法保证编译时让其类型必须相同不然编译不通过呢？有那就是泛型类型。

  简单来说是为了防止**当编译时是Object的对象**，运行时才知道**具体类型的对象强制转换**错误而用泛型来代替。


- **运行时绕过泛型限制**

  ``` java
  public static void main(String args[])
  {
   		List<String> list = new ArrayList<String>();
  		Class sClass = list.getClass();
  		Method me;
  		try {
  			me = sClass.getMethod("add", Object.class);
  			me.invoke(list, 100);//添加Integer类型数据
  			Method method = sClass.getMethod("get", int.class);
  			Object i = method.invoke(list, 0);//获取list.get（0）数据
  			System.out.println(i);//输出
  		} catch (NoSuchMethodException | SecurityException e) {
  			// TODO Auto-generated catch block
  			e.printStackTrace();
  		} catch (IllegalAccessException e) {
  			// TODO Auto-generated catch block
  			e.printStackTrace();
  		} catch (IllegalArgumentException e) {
  			// TODO Auto-generated catch block
  			e.printStackTrace();
  		} catch (InvocationTargetException e) {
  			// TODO Auto-generated catch block
  			e.printStackTrace();
  		}
    
  }
  ```

  从中我们可以看出，泛型并不能限制运行时的对象。

- **泛型‘？’和'E T V'**

  T E V都是一样的只是不同符号也可以用A ,B, C，一般T代表type， E代表集合元素，V代表Value

  **？**是用于限制类型上下边界`List<? extends father>`只要是father的子类都可以,当然也可以不限制类型 比如`ist<?>`。

- **泛型有上下变界**
    1. 上边界是 `<？ extends T>`必须要是T的子类
    2. 下边界是` <？supers T>`必须要是T的父类

- **泛型应用**

  我们来看一个简单泛型应用：

  ```java
  //先定义一个FatherBox接口
  public interface FatherBox {
   void println();
   int getID();
  }
  //定义两个FatherBox的子类
  public class ChildFirstBox implements FatherBox {
  	static int id = 0;
  	private final int _id;
  	public ChildFirstBox() {
  		// TODO Auto-generated constructor stub
  		id++;
  		_id=id;
  	}

  	@Override
  	public void println() {
  		// TODO Auto-generated method stub
  		System.out.println("ChildFirstBox Id:"+_id);
  	}

  	@Override
  	public int getID() {
  		// TODO Auto-generated method stub
  		return _id;
  	}

  }

  public class ChildSecondBox implements FatherBox {
  	static int id = 0;
  	private final int _id;
  	public ChildSecondBox() {
  		// TODO Auto-generated constructor stub
  		id++;
  		_id = id;
  	}

  	@Override
  	public void println() {
  		// TODO Auto-generated method stub
  		System.out.println("ChildSecondBox ID:" + _id);
  	}

  	@Override
  	public int getID() {
  		// TODO Auto-generated method stub
  		return _id;
  	}

  }
  //试运行程序
  	public static void main(String[] args) {
  		ChildFirstBox tBox = new ChildFirstBox();
  		ChildSecondBox tBox2 = new ChildSecondBox();
  		List<FatherBox> as = new ArrayList<>();
  		as.add(tBox);
  		as.add(tBox2);
  		getNumChildFB(as);
  	}

  	public static void getNumChildFB(List<? extends FatherBox> sdf) {
  		for (FatherBox t : sdf) {
  			t.println();
  		}

  	}
  ```


  	  Box 对象可以接受任何FatherBox的子类对象，并正确的输出。从上面的例子中，我们可以看出泛型相当于Object，只是Object无法在编译时指定类型，而泛型可以。

- **泛型优点**

  1. 类型安全

     编译器可以知道当前类型，提高java程序的类型安全

  2. 消除强制类型转化

     消除源代码中的许多强制类型转换

  3. 提高性能

     为以后jvm优化提供了可能性

- **泛型注意事项**

  1. 泛型的类型参数只能是类类型（包括自定义类），不能是简单类型。
  2. 泛型的类型参数可以有多个。
  3. 不能对确切的泛型类型使用instanceof操作。
  4. [不能创建一个确切的泛型类型的数组](http://blog.csdn.net/aabbwoshishei/article/details/50163261)。
