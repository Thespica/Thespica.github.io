---
title: 迭代器
date: 2023-10-31 22:48:00
tags: 
categories: 设计模式
---

## 需求

无需关注细节，即可按照特定顺序依次访问、修改一个集合中的各个元素（这里的修改不包含对集合结构的改动）。

关键概念：

- Iterable：一个按照特定结构（线性表、树、图等）组织了一系列元素的集合，并且该集合有至少一种线性的方式遍历集合内的每一个元素；
- Element：集合中的最小单元；
- Iterator：包含一个访问 Iterable 的元素次序的规则，可以记录某一时刻访问的状态，并可以根据该状态不直接依赖 Iterable 而得到**当前元素**和**下一状态**，能判断是否完成了遍历。

<!--more-->

## for vs. for-each

传统的基于索引的 for 循环也对应有记录数据的集合，和用于记录状态的下标（指针），但有几个不同：

|      |             for              |       for-each        |
| :--: | :--------------------------: | :-------------------: |
| 粒度 |          灵活，繁琐          |      单一，便捷       |
| 移动 | `idx++` 或 `ptr = ptr->next` | `next()` 函数的副作用 |
| 取值 |     `arr[idx]` 或 `*ptr`     | `next()` 函数的返回值 |



## Iterable vs. Iterator

|            Iterable            |                Iterator                |
| :----------------------------: | :------------------------------------: |
| 可以用 for-each 循环遍历的集合 |       可以用于遍历一个集合的接口       |
|   需要实现 `iterator()` 方法   | 需要实现 `next()` 和 `hasNext()` 方法  |
|      存储数据，不存储状态      | 存储状态，可直接取得对应状态的单个数据 |
|  不允许在迭代中改变集合的结构  |       允许在迭代中增添、删除元素       |

## Java 实现

Element 元素类（可以是泛型）：自行实现，无特殊要求。

Aggregate 集合类：继承 `java.lang.Iterable<Element>`，并实现其用于返回迭代器的 `iterator()` 函数：

```java
public class Aggregate implements Iterable<Element> {
    @Override
    public Iterator<Element> iterator() {
        return new ElementIterator();
    }
}
```

Iterator<Eliement> 迭代器类，有取得元素并使迭代器变为下一状态的 `next` 函数，和用于判断是否还有下一个元素的 `hasNext()` 函数：

```java
public class ElementIterator implements Iterator<Element> {
    int current = begin();
    int end = end();

    public Element next() {
        return getNextElement();
    }
    
    public boolean hasNext() {
        return cursor != end;
    }

    private Element getNextElement() {
        ...
    }
}
```

## Python 实现

Element 元素类：无特殊要求。

Iterable 集合类：需要实现其用于返回迭代器的 `__iter()__` 函数。

Iterator 迭代器类：有一个取得元素并使迭代器变为下一状态的 `__next()__` 函数，没有下一个元素时，抛出 `StopIteration` 异常。一个集合和一个迭代器都包含足以完成遍历的信息，即 Iterator 也可以是 Iterable，为了实现这符合直觉的一点，python 要求 Iterator 也拥有 `__iter()__` 函数，一般返回自己（虽然 CPython 自己并不遵守这一点）。

参考：

- [Differences Between Iterator and Iterable and How to Use Them? | Baeldung](https://www.baeldung.com/java-iterator-vs-iterable)
- [Glossary — Python 3.12.0 documentation](https://docs.python.org/3/glossary.html#term-iterable)