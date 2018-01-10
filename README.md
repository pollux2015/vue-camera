# vue-camera
vue-camera，一个自己写的vue插件，getusermedia调用摄像头拍照与生成canvas, 毕设做完会继续填坑~~

### 演示(demo)
[demo](https://pollux2015.github.io/vue-camera/)

### Install
```javascript
npm install vue-camera
```

### How to use

组件调用
```javascript
<template>
  <div>
    <Camera/>
  </div>
</template>

import camera from 'vue-camera'
export default {
  name: 'app',
  components: {
    Camera
  }
}
```

Props
```javascript
props: {
  ow: {
    type: [Number, String],
    default: 400
  },
  oh: {
    type: [Number, String],
    default: 300
  },
  fillColor: {
    type: String,
    default: '#000'
  },
  placement: {
    type: String,
    default: '请点击打开摄像头'
  },
  maxView: {
    type: Number,
    default: 10
  }
}
```

### $emit
```javascript
this.$emit('error', err); // 错误
this.$emit('tocanvas', dataUrl) // 拍照生成canvas
```
