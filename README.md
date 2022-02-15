# React Performance Optimization Study

> 프로젝트를 진행하면서 배우고, 적용시킨 최적화 기법을 정리하기 위해 만들었습니다.

---

## 목차

[1. state의 선언 위치](#1.state의-선언-위치)


## 1.state의 선언 위치
리액트가 리렌더링이 되는 조건 중 하나는 해당 컴포넌트의 `state`가 변경될 때이다. 그렇기 때문에 `state`를 어디에 선언하는지에 따라 성능의 차이를 확인할 수 있다.

예를 들어, 아래와 같은 디렉터리 구조로 간단한 todo앱을 만들었다고 해보자.
```
├── App.jsx
       └── InputBox.jsx
       └── ToDoList.jsx
```

아래는 `App.jsx`의 코드이다.
```Javascript
import React, { useState } from 'react';
import './App.css';

import InputBox from './views/InputBox';
import TodoList from './views/TodoList';

function App() {
  const [text, setText] = useState('');
  const [todoList, setTodoList] = useState([]);
  return (
    <div className="App">
      <InputBox text={text} setText={setText} />
      <TodoList todoList={todoList} />
    </div>
  );
}

export default App;
```

위의 코드처럼 할 일을 입력받아 저장하는 상태인 `text`를 선언하였고 이를 아래와 같은 `InputBox` 컴포넌트에 전달하였다. 
```javascript
const InputBox = ({ text, setText }) => {
  return (
    <>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button>+</button>
    </>
  );
};

export default InputBox;
```

위와 같이 작성했을 때 입력값이 변할 때마다 최상위 컴포넌트에 선언한 상태가 변하면서 리렌더링이 발생한다. 이때의 문제는 `TodoList` 컴포넌트까지 리렌더링 된다는 것이다. 
만약에 서버에서 수많은 데이터를 받아온 후에 렌더링한다면 입력창이 변할 때마다 많은 데이터를 계속 리렌더링해야 할 것이다. 

따라서 아래처럼 `text`상태를 `InputBox`컴포넌트에 선언하고 최종적인 입력 상태만 전달하도록 하는 것이 좋다.
```javascript
import { useState } from 'react';

const InputBox = ({ onClick }) => {
  const [text, setText] = useState('');

  return (
    <>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={() => onClick(text)}>+</button>
    </>
  );
};

export default InputBox;
```
위의 코드처럼 작성하면 입력창이 변할 때 `TodoList`는 리렌더링이 되지 않는다.

[Back to Top](#목차)