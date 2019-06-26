[预览](https://zznire.github.io/bml-activity/)

主要通过 setInterval 来进行 跟随鼠标运动，16ms执行一次，相当于60帧动画。

监听 mouseMove 事件 来 改变目标位置，setInterval 就根据目标位置的改变来 不断运行 run方法

run方法：
```js
CObj.prototype.run = function () {
        if (!this.parent) {
          this.x0 = m.x;
          this.y0 = m.y;
        } else {
          this.x0 = this.parent.x;
          this.y0 = this.parent.y;

        }
        this.x = this.PX += (this.ddx += ((this.x0 - this.PX - this.ddx) + this.cx) / rs) / sp;
        this.y = this.PY += (this.ddy += ((this.y0 - this.PY - this.ddy) + this.cy) / rs) / sp;
        this.css.transform = `translate3D(${Math.round(this.x)}px,${ Math.round(this.y)}px,0)`
      }
```
其中 
* x0,y0 为目标位置
* PX,PY 为上一次运动所在位置
* x,y 为本次运动位置
* sp 为 运动速度，值越大越慢
* rs 为 运动惯性，值越大惯性越大
* cx，cy 为相对于父节点偏移的距离，这里都为0


为了实现 不同的dom 依次开始运动，可以为 dom设置 parent dom，此dom的目标位置 是 父节点本次的运动位置。


## 问题

#### transform
只对 block 或 inlineblock 生效，像`<span>`这种标签不生效

#### vm vh
最小值为1 

# bml_activity

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
