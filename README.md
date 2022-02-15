# React Performance Optimization Study

> 프로젝트를 진행하면서 배우고, 적용시킨 최적화 기법을 정리하기 위해 만들었습니다.

---

## 목차

[1. state의 선언 위치](#1.state의-선언-위치)


## 1.state의 선언 위치
리액트가 리렌더링이 되는 조건 중 하나는 해당 컴포넌트의 `state`가 변경될 때이다. 그렇기 때문에 `state`를 어디에 선언하는지에 따라 성능의 차이를 확인할 수 있다.

예를 들어, 아래와 같은 디렉터리 구조로 간단한 todo앱을 만들었다고 해보자.
```
├── App.tsx
       └── InputBox.tsx
       └── ToDoList.tsx
```

그리고 App.tsx에 `const [text, setText] = useState('')`를 선언하고, `text`와 `setText`를 하위 컴포넌트인 `InputBox.tsx`에 전달한다고 해보자.

이후 `InputBox.tsx`에서 `<input />`의 값이 변할 때마다 `text`를 업데이트해준다면 입력하는 동안 `App.tsx`가 리렌더링될 것이다.

만약 `App.tsx`에서 todo-list를 서버에서 요청하여 수 만개의 데이터를 가져온 후 렌더링한다면 입력할 때마다 리렌더링이 될 것이다.

따라서 `App.tsx`는 입력한 것을 받기만 하면 되기 때문에 `InputBox.tsx`에서 `text`를 선언하여 업데이틀 해주고, 최종적인 입력값을 보내도록 한다.

이처럼 `state`의 위치에 따라서도 웹의 성능을 향상시킬 수 있다.
