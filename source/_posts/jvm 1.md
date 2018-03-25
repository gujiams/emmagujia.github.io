---
title: JVM 1 - start
date: 2018-03-22 20:00:00
categories: jvm
tags:
---
I found out an amazing [JVM tutorial](http://www.waytoeasylearn.com/2016/04/jvm-tutorial.html) *by* [Ashok Kumar ](https://plus.google.com/113338311861115463183) for starter like me. 
This post mainly come form tutorial above.

<!-- more -->
## What is Virtual?
This is not having physical existence.
## What is Virtual machine?
* Virtual machine is a simple software program which simulates the functions of a physical machine.
* This is not having physical existence, but which can perform all operations like physical machines.
* Physical machine whatever it is doing, all those things we can able to do on the virtual machine.
* Best example of virtual machine is calculator in computer; it is worked like physical calculator.<br>
All virtual machines categorized in to 2 types
1. Hardware based or System based Virtual Machine
2. Software based or Application based or process based Virtual Machine

## Hardware based virtual machines
Physically only one physical machine is there but several logical machines we can able to create on the single physical machine with strong isolation (worked independently) of each other. This type of virtual machines is called hardware based virtual machines. 
Main advantage of hardware based virtual machine is effective utilization of hardware resources. Most of the times these types of virtual machines associated with admin role (System Admin, Server Admin etc). Programmers never associated with the hardware based virtual machines. 
Examples of hardware based virtual machines are
1. KVM (Kernel Based VM) for Linux systems
2. VMware
3. Xen
4. Cloud Computing

## Software based virtual machines
These virtual machines acts as run time engine to run a particular programming language applications. Examples of software based virtual machines are 
1. JVM (Java Virtual Machine) acts as runtime engine to run Java applications
2. PVM (Parrot Virtual Machine) acts as runtime engine to run Perl applications
3. CLR (Common Language Runtime) acts as runtime engine to run .NET based applications

## JVM
JVM is the part of JRE and it is responsible to load and run the java class file. The following picture depicts basic architecture of the JVM.
// TO-DO
The first component in JVM is Class Loader Sub System
### Class Loader Sub System
This system is responsible for loading .class file with 3 activities
i. Loading
ii. Linking
iii. Initialization                                                            
#### Loading
Loading means read .class file from hard disk and store corresponding binary data inside method area of JVM. For each .class file JVM will store following information
1. Fully qualified name of class
2. Fully qualified name of immediate parent
3. Whether .class file represents class|interface|enum
4. Methods|Constructors|Variables information
5. Modifiers information
6. Constant Pool information

After loading the class file and store inside method area, immediately JVM will perform one activity i.e., create an object of type java.lang.Class in heap area.
//TO-DO
Created object is not student object or customer object. It is a predefined class “Class” object that is presently in java.lang package. The created object is represents either student class binary information or customer class binary information.
//TO-DO
Here the created class “Class” object is used by programmer. For example,
```java
package com.carrotexpress.test;

public class UserInformation {
    private String memberId;
    private String memberType;

    public String getMemberId() {
        return memberId;
    }

    public void setMemberId(String memberId) {
        this.memberId = memberId;
    }

    public String getMemberType() {
        return memberType;
    }

    public void setMemberType(String memberType) {
        this.memberType = memberType;
    }
}
```

```java
package com.carrotexpress.test;

import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class Test1 {
    public static void main(String[] args) throws ClassNotFoundException{
        Class clazz = Class.forName("com.carrotexpress.test.UserInformation");
        Method[] methods = clazz.getDeclaredMethods();
        for(Method method : methods){
            System.out.println(method);
        }
        Field[] fields= clazz.getDeclaredFields();
        for(Field field:fields){
            System.out.println(field);
        }
    }
}
```
Output is:
```ftl
public void com.carrotexpress.test.UserInformation.setMemberType(java.lang.String)
public java.lang.String com.carrotexpress.test.UserInformation.getMemberId()
public void com.carrotexpress.test.UserInformation.setMemberId(java.lang.String)
public java.lang.String com.carrotexpress.test.UserInformation.getMemberType()
private java.lang.String com.carrotexpress.test.UserInformation.memberId
private java.lang.String com.carrotexpress.test.UserInformation.memberType
```
For every loaded .class file, only one class “Class” object will be created by JVM, even though we are using that class multiple times in our program. 
For example,

```java
package com.carrotexpress.test;

import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class Test2 {
    public static void main(String[] args) throws ClassNotFoundException {
        UserInformation userInformation1 = new UserInformation();
        Class clazz1 = userInformation1.getClass();

        UserInformation userInformation2 = new UserInformation();
        Class clazz2 = userInformation2.getClass();

        System.out.println(clazz1.hashCode());
        System.out.println(clazz2.hashCode());
    }
}
```

Output is:

```ft1
1834188994
1834188994
```

  Here UserInformation class object created two times, but class is loaded only once. In the above program even through we are using UserInformation class multiple times only one class Class object got created.

#### Linking

After “loading” activity JVM immediately perform Linking activity. Linking once again contain 3 activities,

1. Verification
2. Preparation
3. Resolution

Java language is the secure language. Through java spreading virus, malware these kind of this won’t be there. If you execute old language executable files (.exe) then immediately we are getting alert message saying you are executing .exe file it may harmful to your system.

 But in java .class files we never getting these alert messages. What is the reason is inside JVM a special component is there i.e., Byte Code Verifier. This Byte Code Verifier is responsible to verify weather .class file is properly formatted or not, structurally correct or not, generated by valid compiler or not. If the .class file is not generated by valid compiler then Byte Code Verifier raises runtime error java.lang.VerifyError. This total process is done in *verification* activity. 

In *preparation* phase, JVM will allocate memory for class level static variables and assigned default values.

E.g. For int ---> 0, For double ---> 0.0, For boolean ---> false

Here just default values will be assigned and original values will be assigned in initialization phase.

Next phase is *Resolution*. It is the process of replacing all symbolic references used in our class with original direct references from method area.

//TO-DO

For the above class, class loader sub system loads `Resolution.class`, `String.class`, `UserInformation.class` and `Object.class`. Every user defined class the parent class is Object.class so every sub class its parent class must be loaded. The names of these classes are stored in “Constant Pool” of “Resolution” class.

In Resolution phase these names are replaced with Actual references from the method area.

#### Initialization

In Initialization activity, for class level static variables assigns original values and static blocks will be executed from top to bottom.

While Loading, Linking and Initialization if any error occurs then we will get runtime Exception saying java.lang.LinkageError. Previously we discussed about VerifyError. This is the child class of LinkageError.

Types of class loaders in class loader subsystem,

1. Bootstrap class loader/ Primordial class loader
2. Extension class loader
3. Application class loader/System class loader

#####  Bootstrap class loader 

Bootstrap class loader is responsible for to load classes from bootstrap class path. Here bootstrap class path means, usually in java application internal JVM uses rt.jar. All core java API classes like String class, StringBuilder class, StringBuffer class, java.lang packages, java.io packages etc are available in rt.jar. This rt.jar path is known as bootstrap class path and the path of rt.jar is:

`jdk ---> jre ---> lib ---> rt.jar`

This location by default consider as bootstrap class path. This Bootstrap class loader is responsible for loading all the classes inside this rt.jar. This Bootstrap class loader is implemented not in java it is implemented by native languages like C, C++ etc.

##### Extension class loader

The extension class loader is the child of bootstrap class loader. This class loader is responsible to load classes from extension class path:

`jdk ---> jre ---> lib ---> ext ---> rt.jar`

The extension class loader is responsible for loading all the classes present in the ext folder. This Extension class loader is implemented in java only. The class name of extension class loader is:

`sun.misc.Launcher$ExtClassLoader.class`

#####  Application class loader

 The Application class loader is the child of Extension class loader. This class loader is responsible to load classes from Application class path. Application class path means classes in our application (Environment variable class path). It internally uses environment variable path. This Application class loader is implemented in java only. The class name of application class loader is:

`sun.misc.Launcher$AppClassLoader.class`

```java
package com.carrotexpress.test;

public class Test {
    public static void main(String[] args) throws ClassNotFoundException {
        System.out.println(String.class.getClassLoader());
        System.out.println(com.sun.java.accessibility.AccessBridge.class.getClassLoader());
        System.out.println(Test.class.getClassLoader());
    }
}
```

Output is:

```
null	//Because Bootstrap class loader is not java object it is designed with C or C++
sun.misc.Launcher$ExtClassLoader@2077d4de	//Here i found a class AccessBridge.class in %JAVA_HOME%/jre/lib/ext
sun.misc.Launcher$AppClassLoader@18b4aac2      
```

#### How Java Class loader works?

Class loader sub system follows delegation hierarchy algorithm. The algorithm simply looks like as following.

//TO-DO

JVM execute java program line by line. Whenever JVM come across a particular class first JVM will check weather this .class file is already loaded or not. If it is loaded JVM uses that loaded class from method area otherwise JVM will requests class loader sub system to load the .class file then class loader sub system sends that request to application class loader. 

Application class loader won't load that requested class, simply it delegates to extension class loader. Extension class loader also won't load that requested class, simply it delegates to boot strap class loader. Now boot strap class loader search in boot strap class path. If the class is found in boot strap class path then loaded otherwise it delegates to extension class loader.

Now extension class loader searches in extension class path. If the class is found in extension class path then loaded otherwise it delegates to application class loader. Application class loader now searches in application class path.

If the class is found in application class path then loaded. Suppose boot strap class loader unable find, extension class loader unable to find, application class loader unable to find then we will get run time exception called "*ClassNotFound*" exception. This is the algorithm that class loader sub system follows. This algorithm is called “Delegation hierarchy algorithm”.

Here the highest priority will be bootstrap class path, if the class not found in bootstrap class path the next level priority is extension class path, if the class not found in extension class path the next level priority is application class path.

#### Customized class loader

Sometimes we may not satisfy with default class loader mechanism then we can go for Customized class loader. For example

//TO-DO

Default class loader loads .class file only once even though we are using multiple times that class in our program. After loading .class file if it is modified outside , then default class loader won't load updated version of .class file on fly, because .class file already there in method area. To overcome this problem we are going to customized class loader.

//TO-DO

Whenever we are using a class, customised class loader checks whether updated version is available or not. If it is available then load updated version otherwise use already loaded existing .class file, so that updated version available to our program.

CODE

CODE

While designing/developing web servers and application servers usually we can go for customized class loaders to customized class loading mechanism.

### Various Memory Areas in JVM

Whenever a Java virtual machine runs a program, it needs memory to store many things, including byte codes and other information it extracts from loaded class files, objects the program instantiates, parameters to methods, return values, local variables, and intermediate results of computations. The Java virtual machine organizes the memory it needs to execute a program into several runtime data areas.

Various memory areas of JVM are

1. Method Area
2. Heap Area
3. Stack Area
4. PC Registers
5. Native Method Stack

#### Method Area

\* For every JVM one method area will be available

\* Method area will be created at the time of JVM start up.

\* Inside method area class level binary data including static variables will be stored

\* Constant pools of a class will be stored inside method area.

\* Method area can be accessed by multiple threads simultaneously.

\* The size of the method area need not be fixed. As the Java application runs, the virtual machine can expand and contract the method area to fit the application's needs.

\* All threads share the same method area, so access to the method area's data structures must be designed to be thread-safe. 

//TO-DO

#### Heap Area

\* For every JVM one heap area will be available

\* Heap area will be created at the time of JVM start up.

\* Objects and corresponding instance variables will be stored in the heap area.

\* Every array in java is object only hence arrays also will be stored in the heap area.

\* Heap area can be access by multiple threads and hence the data stored in the heap area is not thread safe.

\* Heap area need not be continued.

//TO-DO

##### Display heap memory statistics

 A java application can communicate with JVM by using *Runtime* class object. A *Runtime* class is a singleton class and we can create Runtime object by using `getRuntime()` method.

CODE

 Once we got runtime object we can call the following methods on that object.

1. `maxMemory()`

​     It returns number of bytes of maximum memory allocated to the heap.

2. `totalMemory()`

​     It returns number of bytes of total memory allocated to the heap.

3. `freeMemory()`

​     It returns number of bytes of free memory present in the heap.
E.g

CODE

##### Set Maximum and Minimum heap size

Heap memory is a finite memory based on our requirement we can increase or decrease heap size. We can use following options for your requirement

-Xmx

​     

To set maximum heap size , i.e., maxMemory

​    java -Xmx512m HeapSpaceDemo

​     Here mx = maximum size

​              512m = 512 MB

​              HeapSpaceDemo = Java class name

**-Xms**

​     To set minimum heap size , i.e., total memory 

​        java -Xms65m HeapSpaceDemo   

​      Here ms = minimum size

​              65m = 65 MB

​              HeapSpaceDemo = Java class name

or, you can set a minimum maximum heap size at a time

java -Xms256m -Xmx1024m HeapSpaceDemo

#### Stack Area

#### PC Registers

#### Native Method Stack