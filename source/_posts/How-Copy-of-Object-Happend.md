---
title: 对象的拷贝
date: 2023-04-20 14:53:20
tags:
categories: 
---

# 基本类型的「值拷贝」与「引用拷贝」

对象的状态由基本类型定义而来，搞清基本类型的两种拷贝（传递）方式将有助于推导出对象的传递方式。<!--more-->

## 值拷贝

对于原有的基本类型数据，值拷贝产生一个新的数据。此时新数据与原数据的数值相同，但两者是不同的两个数据，改变其中一个数据的值不会影响另一个数据的值。

## 引用拷贝

引用拷贝不会产生新的数据，而是添加一个对原有数据的访问标识。此时新标识与原有标识指代的是同一个数据，从一个标识改变数据，再从另一个标识访问数据则会得到改变后的数据，因为两个标识指向的是同一个数据。

# 对象的「深拷贝」与「浅拷贝」

此处把对象看作是以基本类型为叶节点的树：要拷贝一颗树，只需依次拷贝子节点。对于子节点的拷贝，仍然可以借用值拷贝与引用拷贝的概念：

* **引用拷贝**并不需要了解自己到底拷贝了什么，毕竟对象就在那里，引用不过是对象的另一个标识；
* **值拷贝**就比较麻烦了，我们发现一个尴尬的问题：要明白如何拷贝对象，就需要明白如何拷贝子节点，如果子节点是对象，那问题又回来了……

好在，子节点是有边界的——引用拷贝本身就可以结束；值拷贝虽然套娃，但一层一层套下去，总能遇见基本类型数据——对此我们是了解的。于是，对象的拷贝可以描述成一种递归：

```
拷贝对象（原对象）:
    遍历原对象中的子节点:
        if 引用拷贝:
            直接引用原有子节点
        else:
            if 子节点是基本类型数据:
                值拷贝
            else:
                拷贝对象（子节点）
```

于是，我们用利用值拷贝和引用拷贝严格地描述了对象的拷贝。实际情况中常常用「深拷贝」和「浅拷贝」来描述：

## 深拷贝

对于原有对象，每一层的子节点都按值拷贝，深度优先搜索整个对象，遇到基本数据则拷贝并退回。

## 浅拷贝

对于原有对象，直属的子节点都按其默认方式拷贝。

---

注意区分：引用对象、浅拷贝和深拷贝。引用是绝对的浅，深拷贝则是绝对的深，唯独浅拷贝的「浅」是有歧义的，某些情况下两个不同的拷贝方法会产生同样的结果。

浅拷贝和深拷贝可能不足以描述所有的情况，只需要记住，值拷贝和引用拷贝就能够严格地描述对象的拷贝。

# 不同语言的拷贝规则

## C

### 基本类型

通过`=`（赋值）或函数间参数传递的拷贝都是值拷贝。

```c
void args_in_func(int arg, int *ptr) {
    printf("argument in function:%p\n"
           "pointer in function:%p\n", &arg, ptr);
}

int mian(void) {
    int ori = 1;
    int copy = ori;
    printf("ori:%p\ncopy:%p",&ori, &copy);
    args_in_func(ori, &ori);
    return 0;
}
```

结果：

```
ori:  000000f30c5ff63c 
copy: 000000f30c5ff638
```

说明 `copy` 是与原数据有相同值的另一个数据。

### 指针

指针是一类特别的数据，用于存放其他数据的地址。C 中不直接提供引用，而是通过指针间接达到引用的效果。

就指针本身而言，赋值和传递都是关于地址的值拷贝；就指针指向的数据而言，则是关于数据的引用拷贝。


```c
void args_in_func(int arg, int *ptr) {
    printf("argument in function:%p\n"
           "pointer in function:%p\n", &arg, ptr);
}

int main(void) {
    int ori = 1;
    int copy = ori;
    printf("ori:%p\ncopy:%p\n",&ori, &copy);
    args_in_func(ori, &ori);
    return 0;
}
```

结果：

```
ori:                  0000001fe19ffc7c                 
copy:                 0000001fe19ffc78                
argument in function: 0000001fe19ffc50
pointer in function:  0000001fe19ffc7c 
```

函数中的参数和原参数是不同的两个变量。对于指针本身也是如此，不过指针保存的地址相同，即可以通过不同的指针访问同一个变量。

### 结构体

结构体默认是将成员变量（按照默认拷贝方式）依次拷贝一遍：基本类型数据是值拷贝，指针是拷贝地址，结构体则拷贝成员。

需要注意，如果原结构体中的指针指向了某个不希望被共用的数据，应当深拷贝这份数据，再用新结构体中的指针指向原数据的深拷贝。

## C++

### 引用

```cpp
void refer_in_func(int &refer) {
    printf("refer in function:%p\n", &refer);
}

int main() {
    int ori = 1;
    int &refer = ori;
    printf("ori:%p\nrefer:%p\n", &ori, &refer);
    refer_in_func(ori);
    return 0;
}
```

结果：

```
ori:               000000fe451ff9d4              
refer:             000000fe451ff9d4            
refer in function: 000000fe451ff9d4
```

引用来引用去，到底是一个东西。

### 类

C++ 中类的默认赋值运算符 `=` 与 C 中的结构体类似：依次复制成员变量，遇到指针不跳转。

```cpp
class Num{
public:
    int num{};
};

int main() {
    Num obj_a;
    Num obj_b = obj_a;
    obj_b.num = 1;
    printf("obj_a:%p\nobj_b:%p\n", &obj_a, &obj_b);
    printf("obj_a.num:%p\nobj_b.num:%p\n", &obj_a.num, &obj_b.num);
}
```

结果：

```
obj_a:000000aadc3ffa6c    
obj_b:000000aadc3ffa68    
obj_a.num:000000aadc3ffa6c
obj_b.num:000000aadc3ffa68
```

也许更常见的是自定义的拷贝：重载 `=` 运算符或者重载一个构造方法。

此外，对象在函数间的传递不值得额外考虑，因为如果要改变对象就直接引用；不改变对象 `const &` 也好处颇多，即使需要副本也可以在函数内部实现。

## Java

Java 中的赋值运算：基本数据类型的都是值拷贝，对象都是引用拷贝。

### 基本类型

```java
public class Test {
    public static void main(String[] args) {
        int num = 0;
        System.out.println("num:" + num);
        int copy = num;
        copy = 1;
        System.out.println("change copy:" + copy + "\nnum:" + num);
    }
}
```

结果：

```java
num:0
change copy:1
num:0
```

### 对象

```java
public class Num {
    public int num;

    public Num(int num) {
        this.num = num;
    }

    public Num() {
        this(0);
    }

    public static void change(Num aNum) {
        aNum.num = 2;
        System.out.println("change in func:" + aNum.num);
    }

    public static void main(String[] args) {
        Num aNum = new Num();
        Num bNum = new Num();
        bNum = aNum;
        bNum.num = 1;
        System.out.println("\naNum.num:" + aNum.num);
        Num.change(aNum);
        System.out.println("change in outside:" + aNum.num);
    }
}
```

结果：

```
aNum.num:1
change in func:2
change in outside:2
```

Java 虽然不能直接看到内存地址，但通过结果可以发现：

* 赋值运算改变基本类型的原有数据；
* 赋值运算改变对象的指向：此处 `bNum` 虽然 `new` 了一个对象，但赋值的操作并不是改变这个对象，而是抛弃这个对象。

## Python

Python 有一个重要的概念，叫作「一切皆对象」。就对象而言，和 Java 一样赋值运算都是引用拷贝。

```python
import copy

a = [1, 2, 3, [4, 5]]
assignment = a
cut = a[:]
factory = list(a)
copy_deepcopy = copy.deepcopy(a)
copy_copy = copy.copy(a)

a[0] = 0
a[3][0] = 0
print("a:{} {}".format(a, id(a)))
print("assignment:{} {}".format(assignment, id(assignment)))
print("cut:{} {}".format(cut, id(cut)))
print("factory:{} {}".format(factory, id(factory)))
print("copy_copy:{} {}".format(copy_copy, id(copy_copy)))
print("copy_deepcopy:{} {}".format(copy_deepcopy, id(copy_deepcopy)))
```

结果：

```
a:             [0, 2, 3, [0, 5]] 2291611479552
assignment:    [0, 2, 3, [0, 5]] 2291611479552
cut:           [1, 2, 3, [0, 5]] 2291611724224
factory:       [1, 2, 3, [0, 5]] 2293335453312
copy_copy:     [1, 2, 3, [0, 5]] 2291611723712
copy_deepcopy: [1, 2, 3, [4, 5]] 2291611724480
```

可以看到，默认的赋值运算是直接引用，而切片方法、工厂方法和 `copy` 模块中的 `copy()` 方法则是浅拷贝，只有`copy` 模块中的 `deepcopy()` 方法是深拷贝。