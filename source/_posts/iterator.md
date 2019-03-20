---
title: 迭代器模式
date: 2019-03-20 21:48:42
tags:
  - iterator
  - 设计模式
categories:
  - 设计模式
---

# 介绍
迭代模式提供一种方法来访问一个容器中各个元素，而又不需要暴露该对象的内部细节（将遍历容器抽取出来）。
通常包含的角色有：
- 迭代器: 定义遍历元素的接口
- 具体迭代器： 实现了迭代器接口
- 容器：容器接口，如List
- 集体容器: 具体实现容器

# 实现

## 迭代器
创建迭代器接口

```java
package iterator;


public interface MyIterator<T> {
    boolean hasNext(); // 判断是否有下一个元素
    T next(); // 返回元素
}
```

## 创建具体迭代器

```java
package iterator;

public class ConcretIterator<T> implements MyIterator<T> {
    private MyList<T> myList; // 包含容器接口，可以接收所有其实现
    private int index = 0; // 定义当前遍历容器的指针

    public ConcretIterator(MyList<T> myList) {
        this.myList = myList;
    }
    @Override
    public boolean hasNext() {
        return index < myList.size();
    }

    @Override
    public T next() {
        return myList.get(index++);
    }
}
```

## 自定义容器接口

```java
package iterator;

public interface MyList<T> {
    // 添加元素
    public void add(T obj);
    // 获取元素
    T get(int index);
    public int size();
    public boolean isEmpty();
    //获取遍历容器的迭代器
    public MyIterator<T> iterator();
}
```

## 创建具体容器实现

```java
package iterator;

import java.util.Arrays;

public class MyArrayList<T> implements MyList<T> {
    private Object[] obj = null;
    private int size = 0;

    MyArrayList() {
        this.obj = new Object[10];
    }
    
    MyArrayList(int capacity) {
        this.obj = new Object[capacity];
    }
    @Override
    public void add(T t) {
        ensureCapacity(size+1);
        obj[size++] = t;
    }

    @Override
    public int size() {
        return size;
    }

    @Override
    public boolean isEmpty() {
        return size == 0;
    }

    @Override
    public MyIterator<T> iterator() {
        return new ConcretIterator<T>(this);
    }

    public T get(int index) {
        return (T)this.obj[index];
    }

    // 当容器塞满时自动扩容
    private void ensureCapacity(int capacity) {
        if(capacity > obj.length) {
            int oldCapacity = obj.length;
            int newCapacity = oldCapacity + (oldCapacity>>1);
            obj = Arrays.copyOf(obj,newCapacity);
        }
    }
}
```

## 测试
```java
package iterator;

public class Test {
    public static void main(String[] args) {
        MyList<String> myList = new MyArrayList<String>();
        myList.add("zhangsan");
        myList.add("lisi");
        myList.add("wangwu");
        myList.add("jack");
        myList.add("rose");
        myList.add("simon");

        MyIterator myIterator = myList.iterator();
        while(myIterator.hasNext()) {
            System.out.println(myIterator.next());
        }
    }
}
zhangsan
lisi
wangwu
jack
rose
simon
```