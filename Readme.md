## 定义

![images](http://github.com/Mercurioooo/reflect/raw/master/images/image-20200129112109746.png)

获取反射的三种方法

1. class.forname
2. xx.class
3. 对象.getclass

### demo

```java
package reflect;
public interface MyInterface {
	void interfaceMethod() ;
}
```

```java
package reflect;
//接口
public interface MyInterface2 {
	void interface2Method() ;
}
```

```java
package reflect;

public class Person  implements MyInterface,MyInterface2{
	private int id;
	private String name ;
	private int age ; 
	public String desc ;
	private Person(String name) {
		this.name = name;
	}
	public Person() {
	}
	public Person(Integer id) {
		this.id = id;
	}
	public Person(int id, String name, int age) {
		this.id = id;
		this.name = name;
		this.age = age;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getName() {
		return name;
	}
	public int getAge() {
		return age;
	}
	private void privateMethod() {
		System.out.println(" private method...");
	}	
	private void privateMethod2(String name) {
		System.out.println(" private method2..."+name);
	}

	public void setAge(int age) {
		this.age = age;
	}
	public static void staticMethod() {
		System.out.println("static  method ...");
	}
	@Override
	public void interfaceMethod() {
		System.out.println("interface  Method....");
	}
	@Override
	public void interface2Method() {
		System.out.println("interface2  Method....");	
	}
}
```

## 获取类

### 1.Class.forName()

```java
//1 Class.forName("全类名")
		try {
			Class<?> Clazz =  Class.forName("reflect.Person") ;
			System.out.println(Clazz);
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
```

### 2.类名.class

```java
//		 2 类名.class
		Class<?> perClazz2 = Person.class ;
		System.out.println(perClazz2);
```

### 3.对象.getClass

```java
//		 3 对象.getClass()
		Person per = new Person();
		Class<?> perClazz3 = per.getClass() ;
		System.out.println(perClazz3);
	}
```

## 获取方法

- 获取所有的 公共的方法
  1. 本类方法,父类方法,接口方法 
  2. 符合访问修饰符规律(public可以,private不行)
- 获取当前类的所有方法
  1. 只能是当前类   
  2. 忽略访问修饰符限制

```java
		Class<?> perClazz =  null ; 
		try {
			perClazz =  Class.forName("reflect.Person") ;
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
		//获取所有的 公共的方法(1.本类 以及 父类、接口中的所有方法  2.符合访问修饰符规律)
		  Method[] methods = perClazz.getMethods();
		  for(Method method:methods) {
			  System.out.println(method);
		  }
		  System.out.println("==========");
		  //获取当前类的所有方法（1.只能是当前类   2.忽略访问修饰符限制）
		  Method[] declaredMethods = perClazz.getDeclaredMethods();
		  for(Method declaredMethod:declaredMethods) {
			  System.out.println(declaredMethod);
		  }
```

## 获取接口

```java
		Class<?> perClazz =  null ; 
		try {
			perClazz =  Class.forName("reflect.Person") ;
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
		
		Class<?>[] interfaces = perClazz.getInterfaces() ;
		for(Class<?> inter:interfaces) {
			System.out.println(inter);
		}
```

## 获取父类

```java
//获取所有的父类
		public static void demo04() {
			Class<?> perClazz =  null ; 
			try {
				perClazz =  Class.forName("reflect.Person") ;
			} catch (ClassNotFoundException e) {
				e.printStackTrace();
			}
			Class<?> superclass = perClazz.getSuperclass() ;
			System.out.println(superclass);
			
		}
```

## 获取构造方法

```java
//获取所有的构造方法
		public static void demo05() {
			Class<?> perClazz =  null ; 
			try {
				perClazz =  Class.forName("reflect.Person") ;
			} catch (ClassNotFoundException e) {
				e.printStackTrace();
			}
			Constructor<?>[] constructors = perClazz.getConstructors() ;
			for(Constructor<?> constructor:constructors) {
				System.out.println(constructor);
				
			}
		}
```

### 获取公共属性

```java
//获取所有的公共属性
		public static void demo06() {
			Class<?> perClazz =  null ; 
			try {
				perClazz =  Class.forName("reflect.Person") ;
			} catch (ClassNotFoundException e) {
				e.printStackTrace();
			}
			//公共属性
			Field[] fields = perClazz.getFields() ;
			for(Field field:fields) {
				System.out.println(field);
			}
			System.out.println("====");
			//所有属性  (属性的 ：公共属性\所有属性的区别 同“方法”)
			Field[] declaredFields = perClazz.getDeclaredFields() ;
			for(Field declaredField:declaredFields) {
				System.out.println(declaredField);
			}
		}
```

## 获取当前反射所代表类（接口） 的对象（实例）

```java
	public static void demo07() throws InstantiationException, IllegalAccessException {
		Class<?> perClazz =  null ; 
		try {
			perClazz =  Class.forName("reflect.Person") ;
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
		
		Object instance = perClazz.newInstance() ;
		Person per = (Person)instance ;
		per.interface2Method(); 
	}
```

## 获取对象实例,并操作该对象

### 1.set

```java
	public static void demo01() throws InstantiationException, IllegalAccessException {
			Class<?> perClazz =  null ; 
			try {
				perClazz =  Class.forName("reflect.Person") ;
			} catch (ClassNotFoundException e) {
				e.printStackTrace();
			}
			Person per = (Person)perClazz.newInstance() ;
			per.setName("zs");
			per.setAge(23);
			System.out.println(  per.getName()+","+per.getAge());
	}
```

### 2.操作属性

使用反射时，如果 因为访问修饰符限制造成异常，可以通过 Field/Method/Constructor.setAccessible(true)

```java
	public static void demo02() throws InstantiationException, IllegalAccessException, NoSuchFieldException, SecurityException, NoSuchMethodException, IllegalArgumentException, InvocationTargetException {
		Class<?> perClazz =  null ; 
		try {
			perClazz =  Class.forName("reflect.Person") ;
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
		//新建实例
		Person per = (Person)perClazz.newInstance() ;
    //获取域
		Field idField = perClazz.getDeclaredField(  "id");
		//访问的是private修饰的id，但是private是私有
		//修改属性的访问权限  
    //使用反射时，如果 因为访问修饰符限制造成异常，可以通过 	
    //Field/Method/Constructor.setAccessible(true)
		idField.setAccessible(true);
		idField.set(per, 1);   //per.setId(1);
		System.out.println( per.getId() );
		
		System.out.println("=====");
    //获取方法(方法名+参数),如果没有参数用null
		Method method = perClazz.getDeclaredMethod("privateMethod", null) ;
		method.setAccessible(true);
		method.invoke(per,null )  ;//方法的调用：invoke()
		
		Method method2 = perClazz.getDeclaredMethod("privateMethod2", String.class) ;
		method2.setAccessible(true);
		method2.invoke(per, "zs") ;
		
	}
```

### 3.操作构造方法

#### 获取公共的构造方法getConstructors

```java
		Constructor<?>[] constructors = perClazz.getConstructors() ;
		for(Constructor constructor:constructors) {
			System.out.println(constructor);
		}
```

#### 获取本类的构造方法getDeclaredConstructors

```java
		Constructor<?>[] constructors = perClazz.getDeclaredConstructors() ;
		for(Constructor constructor:constructors) {
			System.out.println(constructor);
		}
```

#### 指定的构造方法

在反射中，根据类型 获取方法时：基本类型（int、char...）和包装类(Integer,Character)是不同的类型

```java
		Constructor<?> constructor = perClazz.getConstructor(Integer.class) ;
		System.out.println(constructor);
		
		Constructor<?> constructor2 = perClazz.getDeclaredConstructor(String.class) ;
		System.out.println(constructor2);
		//Person per = new Person() ;
		//根据获取的private构造方法，获取对象实例
		constructor2.setAccessible(true);
		Person per3 = (Person)constructor2.newInstance("zs") ;
		System.out.println("per3"+per3);
//因为constructor是 有参构造方法(Integer)，因此需要传入一个Integer值
		Person instance2 = (Person)constructor.newInstance(23) ;
		System.out.println(instance2);
//因为constructor3是 无参构造方法，因此不需要传值
		Constructor<?> constructor3 = perClazz.getConstructor() ;
		Person instance = (Person)constructor3.newInstance( ) ;
		
		System.out.println(instance);
	}
```

#### 动态加载类名和方法

在代码中没有指定加载哪个类,而是从文件读入

![image-20200129130445191](/Users/dylan/Library/Application Support/typora-user-images/image-20200129130445191.png)



```java
	public static void demo04() throws InstantiationException, IllegalAccessException, NoSuchFieldException, SecurityException, NoSuchMethodException, IllegalArgumentException, InvocationTargetException, FileNotFoundException, IOException {
		
    //加载文件
		Properties prop = new Properties();
		prop.load(   new FileReader("class.txt") );
		
    //获取类名和方法名
		String classname = prop.getProperty("classname") ;
		String methodname = prop.getProperty("methodname") ;
		
    //获取类
		Class<?> perClazz =  null ; 
		try {
			perClazz =  Class.forName(classname) ;
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
		
    //获取类中的方法
		Method method = perClazz.getMethod(methodname) ;
		method.invoke( perClazz.newInstance()) ;
		
	}
```

#### 越过类型检查

虽然可以通过反射 访问private等访问修饰符不允许访问的属性/方法，也可以忽略掉泛型的约束；但实际开发 不建议这样使用，可能造成程序的混乱。

```java
		ArrayList<Integer> list = new ArrayList<>() ;
		list.add(123) ;
		list.add(3) ;
		list.add(2) ;
//		list.add("zs") ; //这个不行
		
		Class<?> listClazz = list.getClass() ;
		Method method = listClazz.getMethod("add", Object.class) ;
    //这个就可以了
		method.invoke(list , "zs...") ;
		System.out.println(list);
```

#### 万能的set方法

```java
		Person per = new Person();
		PropertyUtil.setProperty(per, "name", "zs");
		PropertyUtil.setProperty(per, "age", 23);
		Student stu  = new Student() ;
		PropertyUtil.setProperty(stu, "score", 98);
		
		System.out.println(per.getName()+","+per.getAge());
		System.out.println(stu.getScore());
```

 
