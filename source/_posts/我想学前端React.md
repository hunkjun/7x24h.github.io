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

```js
import { useState } from 'react';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function Game() {
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const xIsNext = currentMove % 2 === 0;
  const currentSquares = history[currentMove];

  function handlePlay(nextSquares) {
    const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
  }

  function jumpTo(nextMove) {
    setCurrentMove(nextMove);
  }

  const moves = history.map((squares, move) => {
    let description;
    if (move > 0) {
      description = 'Go to move #' + move;
    } else {
      description = 'Go to game start';
    }
    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}

```

### 相关的链接

next.js 是 react 的框架, 自带路由、认证等功能; 

- react 官网: https://zh-hans.react.dev/learn
- next.js 官方文档: https://www.nextjs.cn/learn/basics/create-nextjs-app/setup 和 https://nextjs.org/learn/react-foundations/what-is-react-and-nextjs
