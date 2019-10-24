---
title: 设计模式 —— 观察者模式
date: 2017-09-06 00:38:45
categories: 编程思路
tags:
- 编程思想
- 观察者模式
---
# 观察者模式

在 HeadFirst 设计模式中，讲述「观察者」模式，用了一个现实作为比喻。

1. 报社的业务就是出版报纸。
2. 向某家报社订阅报纸，只要他们有新报纸出版，就会给你送来。只要你还是他们的订阅用户。
3. 当你不想看这一家报社的报纸时，取消订阅，他们就不会再给你送报纸来。
4. 只要报社还在运营，就会有不同的人向报社订阅报纸和取消订阅。
5. 从这里面就可以读到「报社」就是 Subject 而「订阅者」是Observer的关系。

<!-- more -->

换做程序里来思考 `Observer` 需要向 `Subject` 进行注册（订阅）关系后。
`Subject` 的观察者对象集合里就有了 `Observer` 再在程序运行中 `Subject` 对象有了新的数据就自动会通知 `Observe`r 对象。
可以注册多个不同的 `Observer` 来订阅同一个 `Subject` 在不需要之后也可以取消注册（订阅）关系。
这种关系也称之为「一对多」关系。

订阅者模式简要来说即为「一种一对多的关系，并且可随意注册和取消注册，在注册关系存在时，任何数据都会直接推送给每个订阅者。」

在书中有一段不是很复杂的代码，一共是有 6 个 Java 文件来演示订阅者模式。
示例中，代码是围绕着订阅气象站的气温来描述订阅者模式结构。
上下文关系是，有一个大型气象站会公布气象数据，而对应的小型气象数据表需要从气象站中获取数据，而且气象是会随时变化的。
气象站（主题）每次的气象变化，都会告知小型气象数据表（订阅者）。

## Subject.java
```
/** 
 * 编写 Subject 主题接口
 */
package ObserverDemo;
public interface Subject {
    public void registerObserver(Observer o);
    public void removeObserver(Observer o);
    public void notifyObserver();
}
```

## Observer.java
```
/** 
 * 编写订阅者接口，因为订阅者需要在自己的显示上刷新这些气象数据。
 */
package ObserverDemo;
public interface Observer {
    public void update(float temp, float humidity, float pressure);
}
// DisplayElement.java
/** 
 * 编写显示元素的接口
 */
package ObserverDemo;
public interface DisplayElement {
    public void display();
}
```

## WeatherData.java
```
/** 
 * 编写主题具体逻辑
 */
package ObserverDemo;
import java.util.ArrayList;
public class WeatherData implements Subject {
    private ArrayList observers;
    private float temperature;
    private float humidity;
    private float pressure;
    public WeatherData(){
        observers = new ArrayList();
        // 构造器里会默认创建一个订阅者的数组对象，用于存放多个订阅者
    }
    @Override
    public void registerObserver(Observer o) {
        observers.add(o);
        // 当订阅者注册到该主题时，增加到数组中
    }
    @Override
    public void removeObserver(Observer o) {
        int i = observers.indexOf(o);
        if (i >= 0) {
            observers.remove(i);
        }
        // 当订阅者取消注册时，从数组中移除
    }
    @Override
    public void notifyObserver() {
        for (int i = 0; i < observers.size(); i++){
            Observer observer = (Observer)observers.get(i);
            observer.update(temperature, humidity, pressure);
        }
        // 遍历订阅者数组里的每个订阅者对象，并且调用订阅者的更新数据方法。
    }
    public void measurementsChanged() {
        notifyObserver();
    }
    public void setMeasurements(float temp, float humid, float pres) {
        this.temperature = temp;
        this.humidity = humid;
        this.pressure = pres;
        measurementsChanged();
        // 设置气象站的气象数据，并且通知订阅者获取新数据
    }
}
```

## CurrentConditionsDisplay.java
```
/** 
 * 编写订阅者具体逻辑
 */
package ObserverDemo;
public class CurrentConditionsDisplay implements Observer, DisplayElement {
    private float temperature;
    private float humidity;
    private Subject weatherData;
    public CurrentConditionsDisplay(Subject weatherData){
        this.weatherData = weatherData;
        weatherData.registerObserver(this);
        // 类构造时，订阅者需要知道自己该订阅那个主题，并注册订阅
    }
    @Override
    public void update(float temp, float humidity, float pressure) {
        this.temperature = temp;
        this.humidity = humidity;
        display();
        // 继承了DispalyElement 用于更新对象自身的属性值，并将其打印。
    }
    @Override
    public void display() {
        System.out.println("Current conditions: " + temperature + "F degrees and " + humidity + "% humidity");
    }
}
```

## WeatherStation.java
```
/** 
 * 最终把上面所写的逻辑都组合起来
 */
package ObserverDemo;
public class WeatherStation {
    public static void main(String[] args) {
        WeatherData weatherData = new WeatherData();
        // 创建一个主题
        CurrentConditionsDisplay currentConditionsDisplay = new CurrentConditionsDisplay(weatherData);
        // 创建一个订阅者对象，并且订阅主题
        weatherData.setMeasurements(80,65,30.4f);
        weatherData.setMeasurements(24,34,36.4f);
        weatherData.setMeasurements(54,76,45);
    }
}
```
通过一个简单的程序，陈述了订阅者模式到底是什么概念~
今天的学习也就到这里啦~

注：
这个观察者模式呢，是最初级的观察者模式，主要是为了弄明白什么是观察者模式。
模式中还有很多高级特性没有陈述的，这些需要自己多多研究拉！