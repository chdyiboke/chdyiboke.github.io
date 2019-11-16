#ant design

##如何给 Modal 之类的组件加上 drag 事件?

```code
// 源码
// https://codesandbox.io/s/n3x23qxrm4
// https://codesandbox.io/s/antd-drag-modal-r8fnr (多层)
// https://github.com/xiabingwu/dragm/blob/master/src/index.js

    this.tdom.removeEventListener("mousedown", this.start);
    document.removeEventListener("mouseup", this.docMouseUp);
    document.removeEventListener("mousemove", this.docMove);

  start = event => {
    if (event.button != 0) {
      //只允许左键，右键问题在于不选择conextmenu就不会触发mouseup事件
      return;
    }
    document.addEventListener("mousemove", this.docMove);
    this.position.startX = event.pageX - this.position.dx;
    this.position.startY = event.pageY - this.position.dy;
  };
  docMove = event => {
    const tx = event.pageX - this.position.startX;
    const ty = event.pageY - this.position.startY;
    const transformStr = `translate(${tx}px,${ty}px)`;
    this.props.updateTransform(transformStr, tx, ty, this.tdom);
    this.position.dx = tx;
    this.position.dy = ty;
  };
  docMouseUp = event => {
    document.removeEventListener("mousemove", this.docMove);
  };
```
