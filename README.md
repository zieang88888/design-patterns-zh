# 🏗️ 设计模式速查大全（中文版）

> 设计模式一站速查：23 种 GoF 经典模式 + 8 种常用现代模式，全中文带示例代码（Java/Python/JS 三语言），一图看懂每个模式，面试和工作都能用。

![stars](https://img.shields.io/badge/⭐-给个Star吧-red)
![license](https://img.shields.io/badge/license-MIT-blue)
![status](https://img.shields.io/badge/status-持续更新-green)

## 为什么需要这份速查？

你是不是也有这样的经历：

- 面试被问"设计模式"，**只会说单例和工厂**，其他一概不知？
- 看设计模式的书，**看的时候都懂，写完代码就忘**？
- 23 种模式那么多，**不知道哪些常用、哪些面试必考**？
- 网上资料要么英文、要么代码只有一种语言、**看不懂**？

**设计模式是区分"码农"和"工程师"的分水岭。**

写代码谁都会，但**写出可扩展、可维护的代码**，靠的就是设计模式。

这份速查把设计模式**一次性整理全**：

- ✅ **23 种 GoF 经典模式**：创建型/结构型/行为型全覆盖
- ✅ **8 种现代模式**：单例之外的常用现代实践
- ✅ **三语言示例**：Java / Python / JavaScript 都有
- ✅ **一图看懂**：每个模式都有"是什么→解决什么→怎么用"
- ✅ **面试高频标注**：哪些必考、哪些常考一目了然

## 📚 目录

### 第一篇：创建型模式（5 种）

> 解决"怎么创建对象"的问题，让创建过程更灵活。

#### 1. 单例模式（Singleton）⭐必考

**是什么：** 保证一个类只有一个实例，全局可访问。

**解决什么：** 配置管理、线程池、连接池、日志系统等只需要一个实例的场景。

**Java 实现（双重检查锁）：**
```java
public class Singleton {
    private static volatile Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

**Python 实现：**
```python
class Singleton:
    _instance = None
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

**JS 实现：**
```javascript
const singleton = (function() {
  let instance;
  return function() {
    if (!instance) instance = new Singleton();
    return instance;
  };
})();
```

**要点：** 构造器私有 + 静态方法获取 + 线程安全（双重检查锁）。面试常追问"怎么保证线程安全""怎么防止反射破坏"。

#### 2. 工厂模式（Factory）⭐必考

**是什么：** 定义一个创建对象的接口，由子类决定实例化哪个类。

**解决什么：** 把"创建对象"和"使用对象"解耦，新增产品不用改客户端代码。

**Java 实现：**
```java
interface Shape { void draw(); }

class Circle implements Shape { public void draw() { System.out.println("画圆"); } }
class Square implements Shape { public void draw() { System.out.println("画方"); } }

class ShapeFactory {
    public Shape getShape(String type) {
        if (type.equals("circle")) return new Circle();
        if (type.equals("square")) return new Square();
        return null;
    }
}
// 使用
ShapeFactory factory = new ShapeFactory();
Shape shape = factory.getShape("circle");
shape.draw();
```

**要点：** 简单工厂（一个工厂类）/工厂方法（每个产品一个工厂）/抽象工厂（产品族）。面试常考三者的区别。

#### 3. 抽象工厂模式（Abstract Factory）

**是什么：** 创建一组相关对象的工厂接口。

**解决什么：** 需要保证一组产品（如 Windows 风格按钮+输入框）配套使用时。

#### 4. 建造者模式（Builder）

**是什么：** 一步步构建复杂对象，构建和表示分离。

**解决什么：** 对象有很多可选参数（如点餐、构造请求对象）。

**Java 实现：**
```java
class Pizza {
    private String dough;
    private String sauce;
    private boolean cheese;
    public static class Builder {
        private String dough;
        private String sauce;
        private boolean cheese;
        public Builder dough(String d) { this.dough = d; return this; }
        public Builder sauce(String s) { this.sauce = s; return this; }
        public Builder cheese(boolean c) { this.cheese = c; return this; }
        public Pizza build() { return new Pizza(this); }
    }
}
// 使用
Pizza pizza = new Pizza.Builder().dough("薄底").sauce("番茄").cheese(true).build();
```

**要点：** 链式调用 + 不可变对象。Lombok 的 @Builder 就是这个模式。

#### 5. 原型模式（Prototype）

**是什么：** 用原型实例指定创建对象的种类，通过拷贝创建新对象。

**解决什么：** 创建对象成本高时（数据库查询、复杂初始化），用 clone 复制。

**要点：** 深拷贝 vs 浅拷贝的选择，clone 方法/拷贝构造函数。

### 第二篇：结构型模式（7 种）

> 解决"类和对象怎么组合"的问题，让结构更灵活。

#### 6. 适配器模式（Adapter）⭐常考

**是什么：** 将一个类的接口转换成客户期望的另一个接口。

**解决什么：** 新老系统对接、第三方库接口不匹配。

**Java 实现：**
```java
// 老接口
interface OldSocket { void connectWithOld(); }
// 新接口
interface NewSocket { void connect(); }

// 适配器：把老接口适配成新接口
class SocketAdapter implements NewSocket {
    private OldSocket oldSocket;
    public SocketAdapter(OldSocket old) { this.oldSocket = old; }
    public void connect() { oldSocket.connectWithOld(); }
}
```

**要点：** 类适配器（继承）/对象适配器（组合）。现实例子：充电器转换头、Type-C 转 HDMI。

#### 7. 装饰器模式（Decorator）⭐常考

**是什么：** 动态给对象添加职责，不修改原类。

**解决什么：** 需要灵活扩展功能，避免子类爆炸（如咖啡加糖加奶）。

**Python 实现：**
```python
def log_decorator(func):
    def wrapper(*args, **kwargs):
        print(f"调用 {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@log_decorator
def say_hello(name):
    print(f"你好 {name}")

say_hello("小明")  # 自动打印日志
```

**要点：** Java IO 流（BufferedReader 包 FileReader）、Python 装饰器都是这个模式。

#### 8. 代理模式（Proxy）⭐常考

**是什么：** 为其他对象提供一种代理，控制对这个对象的访问。

**解决什么：** 懒加载、访问控制、日志记录、远程代理。

**Java 实现（静态代理）：**
```java
interface Service { void doWork(); }
class RealService implements Service { public void doWork() { System.out.println("真实服务"); } }

class ProxyService implements Service {
    private RealService real;
    public void doWork() {
        System.out.println("记录日志...");
        if (real == null) real = new RealService();
        real.doWork();
    }
}
```

**要点：** 静态代理/动态代理（JDK Proxy）/CGLIB。Spring AOP 就是动态代理。

#### 9. 外观模式（Facade）

**是什么：** 提供一个统一的高层接口，隐藏子系统复杂性。

**解决什么：** 复杂系统对外只暴露简单接口（如电脑开机只需按电源键）。

**要点：** 和适配器的区别：外观是"简化"，适配器是"转换"。

#### 10. 组合模式（Composite）

**是什么：** 树形结构，叶子节点和组合节点统一处理。

**解决什么：** 目录树、组织架构、UI 组件树。

#### 11. 桥接模式（Bridge）

**是什么：** 抽象和实现分离，两者独立扩展。

**解决什么：** 一个类有多个维度的变化（形状×颜色、设备×遥控器）。

#### 12. 享元模式（Flyweight）

**是什么：** 共享对象，减少内存占用。

**解决什么：** 大量重复对象（字符串常量池、Integer 缓存）。

### 第三篇：行为型模式（11 种）

> 解决"对象之间怎么协作"的问题。

#### 13. 观察者模式（Observer）⭐必考

**是什么：** 一对多依赖，被观察者变化时自动通知观察者。

**解决什么：** 事件监听、消息订阅、数据联动。

**Java 实现：**
```java
import java.util.ArrayList;
import java.util.List;

class WeatherStation {
    private List<Observer> observers = new ArrayList<>();
    private int temperature;

    public void addObserver(Observer o) { observers.add(o); }
    public void setTemperature(int t) {
        this.temperature = t;
        notifyObservers();
    }
    private void notifyObservers() {
        for (Observer o : observers) o.update(temperature);
    }
}

interface Observer { void update(int temperature); }

class PhoneDisplay implements Observer {
    public void update(int t) { System.out.println("手机显示温度：" + t); }
}
```

**要点：** 发布-订阅模式的核心。Vue 响应式、事件总线、消息队列都是这个思想。

#### 14. 策略模式（Strategy）⭐必考

**是什么：** 定义一系列算法，可以互相替换。

**解决什么：** 避免大量 if-else（如支付方式、排序算法、会员折扣）。

**JS 实现：**
```javascript
// 策略对象
const strategies = {
  normal: price => price,
  member: price => price * 0.9,
  vip: price => price * 0.8,
};

// 使用
function calculate(level, price) {
  return strategies[level](price);
}
console.log(calculate('vip', 100)); // 80
```

**要点：** 消除 if-else 的神器。面试常考"怎么用策略模式优化一堆 if-else"。

#### 15. 模板方法模式（Template Method）

**是什么：** 父类定义算法骨架，子类实现具体步骤。

**解决什么：** 流程固定但步骤可变（如做菜流程、数据导入导出）。

#### 16. 责任链模式（Chain of Responsibility）

**是什么：** 请求沿责任链传递，直到有对象处理。

**解决什么：** 审批流、异常处理、过滤器链。

**要点：** Spring MVC 拦截器、Servlet Filter、Netty Pipeline 都是责任链。

#### 17. 迭代器模式（Iterator）

**是什么：** 提供顺序访问集合元素的方式，不暴露内部结构。

**解决什么：** 遍历各种集合（Java Iterator、JS 迭代器、Python for-in）。

#### 18. 状态模式（State）

**是什么：** 对象状态变化时改变行为。

**解决什么：** 订单状态机、流程审批（状态多、逻辑复杂时）。

#### 19. 命令模式（Command）

**是什么：** 把请求封装成对象，支持撤销/重做/队列。

**解决什么：** 编辑器撤销重做、任务队列、宏命令。

#### 20. 备忘录模式（Memento）

**是什么：** 保存对象状态，支持恢复到历史状态。

**解决什么：** 游戏存档、编辑器历史记录。

#### 21. 中介者模式（Mediator）

**是什么：** 通过中介者交互，对象间不直接通信。

**解决什么：** 聊天室、机场塔台、复杂对象网络。

#### 22. 访问者模式（Visitor）

**是什么：** 在不修改类的前提下增加新操作。

**解决什么：** 数据结构稳定但操作多变（编译器 AST 遍历）。

#### 23. 解释器模式（Interpreter）

**是什么：** 定义语言文法并解释执行。

**解决什么：** 正则表达式引擎、SQL 解析、表达式计算。

### 第四篇：现代常用模式（8 种）

#### 24. 发布-订阅模式（Pub/Sub）⭐必考

**是什么：** 发布者和订阅者通过事件中心解耦。

**解决什么：** 跨组件通信、事件总线。

**JS 实现：**
```javascript
class EventBus {
  constructor() { this.events = {}; }
  on(event, fn) { (this.events[event] ||= []).push(fn); }
  emit(event, ...args) {
    (this.events[event] || []).forEach(fn => fn(...args));
  }
  off(event, fn) {
    this.events[event] = (this.events[event] || []).filter(f => f !== fn);
  }
}
```

**要点：** 和观察者的区别：发布订阅有事件中心，观察者直接通知。

#### 25. 依赖注入（DI）

**是什么：** 对象依赖由外部容器注入，不在内部创建。

**解决什么：** 解耦、方便测试（Spring IOC 核心）。

#### 26. 工厂 + 策略组合

**是什么：** 工厂创建策略，策略处理业务。

**解决什么：** 复杂业务分发（支付工厂 + 各支付策略）。

#### 27. 空对象模式（Null Object）

**是什么：** 用空对象代替 null，避免判空。

**解决什么：** 消除大量 if (x != null) 判断。

#### 28. 对象池模式（Object Pool）

**是什么：** 复用对象，避免频繁创建销毁。

**解决什么：** 数据库连接池、线程池。

#### 29. 懒加载（Lazy Loading）

**是什么：** 需要时才创建/加载。

**解决什么：** 性能优化（图片懒加载、路由懒加载）。

#### 30. 防抖/节流（Debounce/Throttle）

**是什么：** 控制函数执行频率。

**解决什么：** 搜索联想、滚动加载、resize 监听。

#### 31. 管道模式（Pipeline）

**是什么：** 数据按顺序经过多个处理阶段。

**解决什么：** 数据清洗、中间件、流程处理。

## 🚀 快速开始

```bash
# 直接浏览
# 打开 README.md 即可查看全部内容

# 或者克隆到本地
git clone https://github.com/zieang88888/design-patterns-zh.git
```

## 🤝 贡献

- 有更好的示例代码？提 PR
- 有想补充的模式？提 Issue
- 觉得有用？点个 ⭐ Star 就是最大的支持！

## 📄 License

MIT

---

**如果这份速查帮到了你，请点个 ⭐ Star，让更多程序员告别"只会单例和工厂"！** 🙏

**关注我 [@zieang88888](https://github.com/zieang88888)，持续输出中文开源干货！**