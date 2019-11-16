# JavaScript

基本类型： null/undefined/string/number/boolean/symbol  
引用类型： object/function/array  

object的索引值为字符串！！！number会被转为string。   
array的索引值为数组和字符串。 

```
<!-- 对象的索引值应该即可以是字符串，也可以是数字。当然，如果是数字就必须用[]进行访问，不能用.进行访问而已。 -->
var a = {1:'sd'}
a["1"] // 'sd'
a[1] // 'sd'
a.1 // Uncaught SyntaxError: Unexpected number


// 数组
var arr = [1,2,3];
arr[6] = 1;
arr; // (7) [1, 2, 3, empty × 3, 1]

arr['a'] = 'a';
arr; // (7) [1, 2, 3, empty × 3, 1, a: "a"]

arr['4'] = 'a';
arr; // (7) [1, 2, 3, "a",empty × 2, 1, a: "a"]
```

## 数组

## 对象




