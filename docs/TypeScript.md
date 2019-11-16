#TypeScript

安装TypeScript

```javascript
npm install -g typescript
```
编译一个 TypeScript 文件：
```javascript
// tsc hello.tsx   react
tsc hello.ts
```

字面量、元组、枚举
字面量
```javascript
// 基础语法
type EventNames = 'click' | 'touch';
```
元组
```javascript
// 基础语法
let persion: [string, number] = ['tom', 24];
// 函数式编程中定义参数和返回值
```
枚举
```javascript
// 基础语法
enum Days { Sun, Mon, Tue, Wed, Thu, Fri, Sat };
// 代码中的各种0123，你还记得是什么意思吗，用枚举再也忘不掉--语义化别名
```

字面量 vs 枚举  
    枚举是给一些没有意义、难以记忆的id一个别名  
    字面量是已知字符串的集合  
元组 vs 数组  
    元组：不同类型元素   
    数组：相同类型的元素  


