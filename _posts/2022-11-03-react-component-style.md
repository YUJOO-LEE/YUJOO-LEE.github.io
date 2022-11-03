---
layout: post
title:  react Function 컴포넌트, Class 컴포넌트
date:   2022-11-03 23:17:02 +0900
comments : true
categories: Review
tags: [inflearn, react]
---

Function 컴포넌트와 Class 컴포넌트의 차이는 기존에 궁금했던 부분이어서 강의를 찾아보고, 검색을 통해 추가적인 내용을 보충해서 이해했다.

[인프런 - React class vs. function style coding](https://www.inflearn.com/course/react-class-function-%EC%83%9D%ED%99%9C%EC%BD%94%EB%94%A9) 

class형 보다 함수형 컴포넌트가 사용이 더 쉽지만, 예전에는 class형 컴포넌트만 state나 react life cycle을 이용할 수 있었기 때문에 대부분 class 문법으로 작성이 되었다.

하지만 Hook이 등장하면서 함수형에서도 state나 react life cycle을 이용할 수 있게 되어서 함수형으로도 많이 쓰인다.

<br><br>
<hr>
<br><br>

### 컴포넌트 생성

#### function 컴포넌트

```javascript
function FuncComp(){
  return(
    <h1>
      함수형 컴포넌트
    </h1>
  );
}
```

<br>

#### class 컴포넌트

```javascript
class ClassComp extends Component{
  render(){
    return (
      <h1>
        클래스형 컴포넌트
      </h1>
    );
  }
}
```

`render()` 메서드를 이용해 컴포넌트를 생성한다.

<br><br>
<hr>
<br><br>

### props

상위 컴포넌트에서 props 값을 넘길 때 하위 컴포넌트에서 받는 법의 차이가 있다.

#### function 컴포넌트

```javascript
function FuncComp(props){
  return(
    <h1>
      {props.이름}
    </h1>
  );
}
```

파라미터로 props를 정의해서 사용한다.

<br>

#### class 컴포넌트

```javascript
class ClassComp extends Component{
  render(){
    return (
      <h1>
        {this.props.이름}
      </h1>
    );
  }
}
```

class 객체로 자동으로 넘어와서 this.props 로 호출한다.

<br><br>
<hr>
<br><br>

### state

#### function 컴포넌트

`useState()` Hook을 import 해서 사용한다.

```javascript
function FuncComp(props){

  const [ 이름, set이름 ] = useState('값');

  return(
    <h1>
      {이름}
    </h1>
  );
}
```

<br>

#### class 컴포넌트

**방법1** constructor 로 state를 생성한다.

이 때 this.props 의 정의를 위해 `super()` 선언을 해 주어야 한다.

이때 들어가는 값이 state의 초기값이다.

```javascript
class ClassComp extends Component{

  constructor(props) {
    super(props);
    this.state = { 이름: '값' };
  }

  render(){
    return (
      <h1>
        {this.state.이름}
      </h1>
    );
  }
}
```

<br>

**방법2** class field 에 변수를 선언한다.

ES6 이후 함수와 변수의 선언을 해당 인스턴스 bind를 기본으로 해서 작성할 수 있다.

변수 선언은 `const` , `let` 없이 바로 선언하면 되며, 함수는 `화살표함수 ()=>{}` 의 형태여야 한다.

```javascript
class ClassComp extends Component{

  state = { 이름: '값' }

  render(){
    return (
      <h1>
        {this.state.이름}
      </h1>
    );
  }
}
```

<br><br>
<hr>
<br><br>

### 함수 호출

#### function 컴포넌트

state가 개별적인 변수로 호출이 되어 있으므로 (this 를 사용하는 것이 아니므로) 일반적으로 호출한다.

```javascript
function FuncComp(props){

  const [ 이름, set이름 ] = useState('값');

  return(
    <button onClick={function(){
      console.log(이름);
    }}>
      클릭하세요
    </button>
  );
}
```

<br>

#### class 컴포넌트

class 내부의 프로퍼티를 사용하므로 this를 바인딩 해 주어야 한다.

그냥 호출할 경우 this가 전역인 window를 바라보면서 해당 프로퍼티를 찾지 못한다.

```javascript
class ClassComp extends Component{

  state = { 이름: '값' }

  render(){
    return(
      <button onClick={function(){
        console.log(this.state.이름);
      }.bind(this)}>
        클릭하세요
      </button>
    );
  }
}
```

<br><br>
<hr>
<br><br>

### life cycle

#### function 컴포넌트

`useEffect()` Hook 을 import 해서 사용한다.

useEffect() 의 첫번째 인자로는 호출할 함수를 입력하고, 두번째 인자로는 배열의 형태로 state를 넣을 수 있다.

배열이 없으면 마운트, 업데이트 직전에 해당 함수를 실행하고, 빈 배열이면 마운트 시에만 실행된다.

배열에 state가 있으면, 마운트시와 해당 state의 값 업데이트 시에만 실행된다.

state가 여러개인데 배열로는 하나의 state 만 지정하면, 지정한 state를 제외한 나머지 state가 변경 될 때에는 실행되지 않는다.

```javascript
function FuncComp(props){

  useEffect(()=>{
    console.log('마운트 or 업데이트 되었습니다.');

    return(()=>{
      console.log('언마운트 or Did업데이트 되었습니다.');
    });
  });

  return(
    <h1>
      내용
    </h1>
  );
}
```

위 컴포넌트가 마운트 또는 업데이트되면 콘솔창에는 '마운트 되었습니다.' 가 출력된다.

마운트 시에만 호출하고 싶다면 `useEffect(함수, []);` 의 형태로 빈 배열을 넣어준다.

만약 언마운트 또는 Did업데이트 시 함수를 실행하고 싶다면 `useEffect(함수)` 의 함수 내부에서 언마운트시 실행할 함수를 return 하면된다.

위 컴포넌트의 언마운트 또는 Did업데이트 시에는 콘솔에 '언마운트 되었습니다.' 가 출력 될 것이다.

<br>

#### class 컴포넌트

class 컴포넌트는 `render()` 실행 전, 즉 컴포넌트가 마운트 되기 전에 `componentWillMount()` 를 먼저 실행하고, `render()` 실행 후에, 즉 마운트가 되고 난 후에 `componentDidMount()` 를 실행해서 마운트에 대해 원하는 타이밍에 원하는 코드를 적용할 수 있다.

```javascript
class ClassComp extends Component{

  componentWillMount(){
    console.log('마운트 되었습니다.');
  }
  componentDidMount(){
    console.log('마운트가 완료되었습니다.');
  }
  render(){
    console.log('렌더 되었습니다.');

    return(
      <h1>
        내용
      </h1>
    );
  }
}
```

위 컴포넌트가 마운트되면 콘솔창에는 '마운트 되었습니다.', '렌더 되었습니다.', '마운트가 완료되었습니다.' 가 차례로 출력된다.

언마운트 시에 함수를 호출하고 싶다면 `componentWillUnmount()` 를 사용한다.

<br>

마운트가 아닌 재렌더(업데이트)에 대해 코드를 실행하고 싶다면 `shouldComponentUpdate()`, `componentWillUpdate()`, `componentDidUpdate()` 를 사용한다.

```javascript
class ClassComp extends Component{

  shouldComponentUpdate(nextProps, nextState){
    console.log('업데이트 여부를 결정합니다.');
    return true;
  }
  componentWillUpdate(nextProps, nextState){
    console.log('업데이트를 실행합니다.');
  }
  componentDidUpdate(prevProps, prevState){
    console.log('업데이트가 완료되었습니다.');
  }
  render(){
    console.log('업데이트 되었습니다.');

    return(
      <h1>
        내용
      </h1>
    );
  }
}
```

`shouldComponentUpdate()` 에서 업데이트 조건을 지정하고, `return` 하는 boolean 값에 따라 업데이트를 실행 여부가 결정된다.

Will 및 Did 는 마운트에 대한 메서드와 비슷하다.

위 코드 실행 후 state 값이 변경되면 콘솔에는 '업데이트 여부를 결정합니다.', '업데이트를 실행합니다.', '업데이트 되었습니다.', '업데이트가 완료되었습니다.' 순으로 호출된다.

<br><br>
<hr>
<br><br>

Hook 이 생기면서 Function 형 컴포넌트도 Class형 컴포넌트와 비슷한 효과를 낼 수 있게 되었다.

필요에 따라서 자신만의 custom Hook도 만들 수 있다고 한다.

정말 재밌는 기능이다.

<br>
