# 技术分享

## 「Tech Salon 001」技术沙龙

### Python在猫眼运维中的应用

>OPS（腾讯云） -> 美大阿凡达（美大云）
机器管理

>ORG(中间件)
信息安全
美大ORG(thrift


>SSO 


>jumper（运维堡垒机
伪终端
只支持IP登录，域名解析后续支持

Python的特点，解释性语言，适合在内部系统使用，不适合数据量太大的系统。




## 周四typescript分享

### TypeScrip类, 类与接口 -- 聂玉玲、温韵燕

>抽象类与接口的区别

接口--为了规范  
接口可以多实现（只用类型属性、类型不冲突可以赋值。  
类不能多继承- 重复的方法和属性   
抽象类必选被实现  

子类有必选参数 - 接口  
有需要公共实现的方法 - 抽象类  
需要原型链判断类型 - 抽象类  
需要使用访问修饰符 - 抽象类  
问大佬  


```bash
// abstract class
abstract class Department {
  constructor(public name: string) {}
  printName() {
    console.log(`department name: ${this.name}`);
  }
  abstract printMeeting(): void; // 抽象函数必须在派生类中实现
}

class BusinessDepartment extends Department {
  constructor() {
    super('meeting at 11:00');
  }
  printMeeting(): void{
    console.log(`printMeeting here`);
  }
  doPlan(): void{
    console.log(`doPlan here`);
  }
}

let department: Department;  // 注意这一行code
department = new Department(); // err: 无法创建抽象类的实例
department = new BusinessDepartment();
department.printName();
department.printMeeting();
department.doPlan(); // err: 类型“Department”上不存在属性“doPlan”


```


### 泛型类, this 类型 -- 吴楠、陈琳

>泛型类

```javascript

class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };

```


```javascript

// # this 返回用法。  类似jq链式调用
class BasicCalculator {
  public constructor(protected value: number = 0, protected nextValue: number = 1) { }
  
  public currentValue(): number {
    return this.value + this.nextValue;
  }
  
  public add(this: this, operand: number): this {
    this.value += operand;
    return this;
  }
  
  public subtract(operand: number) {
    this.value -= operand;
    return this;
  }
  
  public multiply(operand: number) {
    this.value *= operand;
    return this;
  }
  
  public divide(operand: number) {
    this.value /= operand;
    return this;
  }
}

let value = new BasicCalculator(2)
    

let multiply = value.multiply
multiply(2)
console.log(value)

class ScientificCalculator extends BasicCalculator {
  public constructor(value = 0) {
    super(value);
  }
  
  public square() {
    this.value = this.value ** 2;
    return this;
  }
  
  public sin() {
    this.value = Math.sin(this.value);
    return this;
  }
}

let newValue = new ScientificCalculator(0.5)
    .add(1)
    .square()
    .divide(2)// 返回值类将被推断为BasicCalculator
    .sin()    // 将会报错，因为BasicCalculator类不具有此方法
    .currentValue();

console.log(newValue)

```

wiki 有查看访问者的功能 666   





### 泛型类, this 类型 -- 吴楠、陈琳

【Tech Show 007】课程提醒！
课题：《前端基础服务的演化之路》
讲师：刘振利
时间：2019年11月05日（今天）15:00
地点：特洛伊会议室

绘图服务

https://github.com/Automattic/node-canvas

api 
截图服务 ---  图片对比

应用：
赞否、微看板、猫眼小程序（运营活动）、通告单小程序、猫眼通（截图-图片对比服务）


规划

总结






### 技术答辩-star模型（量化）
规划：keeper 用户自定义样式-编辑样式

问题
1、重点讲-

样式定制-- 展开。
pc i 小程序 - 区别
结果

总结成长ß
独立负责业务




许伟

















