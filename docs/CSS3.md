# CSS3

## 整体样式
```

cursor:pointer;   // css实现鼠标悬停，光标出现小手
https://blog.csdn.net/lpch0825/article/details/79330682

```


## 0.5px的问题
https://linghs.github.io/Practical-notes/CSS/%E7%A7%BB%E5%8A%A8%E7%AB%AF%E7%82%B95px.html

```
// 解决方法：在dpr为2的前提下，使用transform缩放特性在伪元素中,现将width和height放大2倍，再缩小1/2。
.box {
  position: relative;
  width: 0.84rem;
  line-height: 0.3rem;
  text-align: center;
  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 200%;
    height: 200%;
    border: 1px solid #CCCCCC;
    border-radius: 1rem;
    transform-origin: 0 0;
    transform: scale(0.5, 0.5);
    box-sizing: border-box;
    }
}


```