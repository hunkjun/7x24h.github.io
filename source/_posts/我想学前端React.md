---
title: 我想学前端React
date: 2024-12-03 09:41:22
tags: React
---

### 为什么想学

想做一个web页面的系统;

之前也看过，但总感觉入不了门，可能是自己的方法不对，可能是自己没有用心，也没有耐心，没有坚持!

### 怎么学

每天关注下react官方文档，看官方文档, 深入概念，实际操作; 笨办法学习，每天看一天，重复的看，记录到blog.

后面找一个项目做一下，实践；

**灵码加持，事半功倍!!**

### react 的一些基础概念

1. DOM
2. JSX
3. Babel
4. Components(组件)
5. Props(道具)
6. State(状态)
7. Hook -- 以 use 开头的函数被称为 Hook。useState 是 React 提供的一个内置 Hook。

### 第一个组件

- 确保Node.js已安装;
- npx create-react-app test001 #创建项目
- npm install #安装依赖包
- npm start #启动项目

```js
function Mybutton(props) {
  return (
    <button>
      {props.text}
    </button>
  );
}

export default function App() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <Mybutton text="I am a button" />
    </div>
  );
}
```

### 井字棋项目

#### 构建思路

- 构建棋盘
- props传递参数
- useState记录点击的状态
- 交替放置“X”和“O” , 初始化: Array(9).fill(null)
- 获胜规则
- 时间旅行，历史记录

### 相关的链接

next.js 是 react 的框架, 自带路由、认证等功能; 

- react 官网: https://zh-hans.react.dev/learn
- next.js 官方文档: https://www.nextjs.cn/learn/basics/create-nextjs-app/setup 和 https://nextjs.org/learn/react-foundations/what-is-react-and-nextjs
