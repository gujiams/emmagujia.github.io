---
title: JVM 1 - start
date: 2018-03-22 20:00:00
categories: jvm
tags:
---
I found out an amazing [JVM tutorial](http://www.waytoeasylearn.com/2016/04/jvm-tutorial.html) for starter like me. 
This post mainly come form tutorial above.  

<!-- more -->
### What is Virtual?
This is not having physical existence.
### What is Virtual machine?
* Virtual machine is a simple software program which simulates the functions of a physical machine.
* This is not having physical existence, but which can perform all operations like physical machines.
* Physical machine whatever it is doing, all those things we can able to do on the virtual machine.
* Best example of virtual machine is calculator in computer; it is worked like physical calculator.<br>
All virtual machines categorized in to 2 types
1. Hardware based or System based Virtual Machine
2. Software based or Application based or process based Virtual Machine

### Hardware based virtual machines
Physically only one physical machine is there but several logical machines we can able to create on the single physical machine with strong isolation (worked independently) of each other. This type of virtual machines is called hardware based virtual machines. 
Main advantage of hardware based virtual machine is effective utilization of hardware resources. Most of the times these types of virtual machines associated with admin role (System Admin, Server Admin etc). Programmers never associated with the hardware based virtual machines. 
Examples of hardware based virtual machines are
1. KVM (Kernel Based VM) for Linux systems
2. VMware
3. Xen
4. Cloud Computing

### Software based virtual machines
These virtual machines acts as run time engine to run a particular programming language applications. Examples of software based virtual machines are 
1. JVM (Java Virtual Machine) acts as runtime engine to run Java applications
2. PVM (Parrot Virtual Machine) acts as runtime engine to run Perl applications
3. CLR (Common Language Runtime) acts as runtime engine to run .NET based applications

### JVM
JVM is the part of JRE and it is responsible to load and run the java class file. The following picture depicts basic architecture of the JVM.
// TO-DO
The first component in JVM is Class Loader Sub System
#### Class Loader Sub System
This system is responsible for loading .class file with 3 activities
i. Loading
ii. Linking
iii. Initialization                                                            
##### Loading
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

#####  Linking

After “loading” activity JVM immediately perform Linking activity. Linking once again contain 3 activities,

1. Verification
2. Preparation
3. Resolution

Java language is the secure language. Through java spreading virus, malware these kind of this won’t be there. If you execute old language executable files (.exe) then immediately we are getting alert message saying you are executing .exe file it may harmful to your system.