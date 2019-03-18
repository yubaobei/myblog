---
title: 装饰设计模式
date: 2019-03-18 21:00:35
tags:
  - decorator
  - 设计模式
---

# 介绍
装饰模式可以在不改变现有对象的情况下给对象添加新的功能。虽然继承也可以扩展类的功能，但是随着扩展功能的增多，子类越发“膨胀”，而且通过继承扩展的功能是静态的。使用装饰模式可以灵活的扩展类的功能。Java I/O中就使用到了装饰设计模式。
什么时候使用装饰模式：
1）扩展一个类的功能
2)动态的增加功能，动态的撤销


# 实现
创建一个组件接口（规范被装饰的对象）
```
package decorator;

/**
 * 创建抽象接口，用来规范即将被增强功能的对象（如IO中的InputStream）
 */

public interface Component {
    public void methordA();
}

```

创建具体实现类
```
package decorator;

/**
 * 具体被增强的类（如I/O中的FileIputStream）
 */

public class ConcreteComponent implements Component {

    @Override
    public void methordA() {
        System.out.println("Doing something with A");
    }
}
```

创建装饰器接口
```
package decorator;

/**
 * 装饰器接口，实现了被装饰类的规范，且内部包含一个Component 实例（如I/O 中的FilterInputStream）
 */
public abstract class Decorator implements Component {
    private Component component;

    public Decorator(Component component) {
        this.component = component;
    }

    @Override
    public void methordA() {
        this.component.methordA();
    }
}
```

创建具体装饰器实现类1
```
package decorator;

/**
 * 具体装饰器实现类（如I/O中的BufferedInputStream）
 */
public class ConcreteDecoratorOne extends Decorator {
    public ConcreteDecoratorOne(Component component) {
        super(component);
    }

    @Override
    public void methordA() {
        super.methordA();
        methordB();
    }

    public void methordB() {
        System.out.println("Doing something with B");
    }
}
```

创建具体装饰器实现类2
```
package decorator;

/**
 * 具体装饰器实现类（如I/O中的DataInputStream）
 */
public class ConcreteDecoratorTwo extends Decorator {
    public ConcreteDecoratorTwo(Component component) {
        super(component);
    }

    @Override
    public void methordA() {
        super.methordA();
        methordC();
    }

    public void methordC() {
        System.out.println("Doing something with C");
    }
}
```
测试类
```
package decorator;

public class Test {
    public static void main(String[] args) {
        Component component = new ConcreteComponent();
        // 调用未增强的方法
        component.methordA();
        // Doing something with A

        // 使用装饰器1增强原来的方法
        ConcreteDecoratorOne concreteDecoratorOne = new ConcreteDecoratorOne(component);
        concreteDecoratorOne.methordA();
        // Doing something with A
        // Doing something with B

        // 在装饰器1的基础上继续增强原方法
        ConcreteDecoratorTwo concreteDecoratorTwo = new ConcreteDecoratorTwo(concreteDecoratorOne);
        concreteDecoratorTwo.methordA();
        // Doing something with A
        // Doing something with B
        // Doing something with C
    }
}
```