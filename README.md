# vue-digital-flop

## 介绍
基于Vue3.0的数字翻牌器
## 安装
1. npm
    ```
    npm install vue-digital-flop --save
    ```
2. Yarn
    ```
    yarn add vue-digital-flop
    ```
## 组件配置
| 参数 | 说明                        | 类型      | 默认值  |
| --- |---------------------------|---------|------|
| start | 起始值 | Number | 0    |
| end | 结束值 | Number | 0    |
| duration | 动画持续时间 | Number | 1000 |
| separator | 分隔符 | String | , |
| decimal | 小数点 | String | . |
| prefix | 前缀 | String |  |
| suffix | 后缀 | String |  |
| toFixedNum | 小数点后保留几位 | Number | 0 |


## 注意项
1. 起始值只有在第一次加载时有效，后续更新不会生效
## 使用
### 全局注册
```javascript
// **main.js**
  import { createApp } from 'vue';
  import App from './App.vue';
  import VueDigitalFlop from 'vue-digital-flop'
  const app = createApp(App);
  app.use(VueDigitalFlop);
  app.mount('#app');
```
### 局部注册
```javascript
// **.vue**
  import { VueDigitalFlop } from 'vue-digital-flop'
  export default {
    components: {
       VueDigitalFlop
    }
  }
```
