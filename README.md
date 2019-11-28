# React_Redux_Study1
리액트에서 리덕스에 대한 기본개념을 공부해보았습니다.
## 공부 내용 정리
리덕스란? 

가장 많이 사용하는 리액트 상태 관리 라이브러리이다. 이것을 사용하면 전역 상태를 관리할 때 매우 효과적입니다.

​

리덕스의 주요 개념들은?

​

1. 액션

2. 액션 생성함수

3. 리듀서

4. 스토어

5. 디스패치

6. 구독

​

1. 액션이란?

- 상태에 어떠한 변화가 필요하면 액션(action)이란 것이 발생한다. 이는 하나의 객체로 표현이 됩니다.

​

{

 type: 'TOGGLE_VALUE'

}

​

액션 객체는 type 필드를 반드시 가지고 있어야 합니다. 이 값을 액션의 이름이라고 생각하면 되고 그 외의 값들은 나중에 상태 업데이트를

할 때 참고해야 할 값이며, 작성자 마음대로 넣을 수 있다.

​

{ 

 type: 'ADD_TODO',

 data: {

    id: 1,

    text: '리덕스 배우기'

  }

}

​

{

  type: 'CHANGE_INPUT',

  text: '안녕하세요'

}

​

액션 이름은 주로 문자열 형태로, 주로 대문자로 작성하며 액션 이름은 고유해야 합니다. 이름이 중복되면 의도하지 않은 게 발생하기 때문입니다.

​

const TOGGLE_SWITCH = 'TOGGLE_SWITCH';

const INCREASE = 'INCREASE';

const DECREASE = 'DECREASE';

​

​

​

2. 액션 생성 함수란?

​

액션 생성 함수는 액션 객체를 만들어주는 함수입니다. 

이 함수를 작성할 때에는 type값을 반드시 갖고 있어야 하며, 그 외에 추후 상태를 업데이트할 때 참고하고 싶은 값은 마음대로 넣을 수 있습니다.

​

const toggleSwitch = () => ({ type: TOGGLE_SWITCH });

const increase = difference => ({ type: INCREASE, difference });

const decrease = () => ({ type: DECREASE });

​

​

3. 리듀서 함수란?

​

리듀서는 변화를 일으키는 함수입니다. 액션을 만들어서 발생시키면 리듀서가 현재 상태와 전달받은 액션 객체를 파라미터로 받아오고,

그리고 그 두 값을 참고하여 새로운 상태를 만들어 반환해줍니다.

​

리듀서 코드는 다음과 같은 형태로 이루어져 있습니다.

​

// state가 undefined일 때는 initialState를 기본값으로 사용

function reducer(state = initialState, action) {

    // action.type에 따라 다른 작업을 처리함

    switch (action.type) {

        case TOGGLE_SWITCH:

            return {

                ...state, // 불변성 유지를 해 주어야 합니다.

                toggle: !state.toggle,

            };

        case INCREASE:

            return {

                ...state,

                counter: state.counter + action.difference,

            };

        case DECREASE:

            return {

                ...state,

                counter: state.counter - 1,

            };

        default:

            return state;

    }

}

​

​

4. 스토어란?

​

프로젝트에 리덕스를 적용하기 위해서 스토어(store)를 만듭니다. 한 개의 프로젝트는 단 하나의 스토어만 가질 수 있습니다.

스토어 안에는 현재 애플리케이션 상태와 리듀서가 들어가 있으며, 그 외에도 몇 가지 중요한 내장함수를 지닙니다.

​

스토어는 이렇게 사용합니다.

​

import { createStore } from 'redux';

(...)

const store = createStore(reducer);

​

​

5. dispatch란?

​

디스패치(dispatch)는 스토어의 내장 함수 중 하나입니다. 디스패치는 '액션을 발생시키는 것'이라고 이해하면 됩니다.

이 함수는 dispatch(action)과 같은 형태로 액션 객체를 파라미터로 넣어서 호출합니다.

​

이 함수가 호출되면 스토어는 리듀서 함수를 실행시켜서 새로운 상태를 만들어 줍니다.

​

divToggle.onclick = () => {

    store.dispatch(toggleSwitch());

};

btnIncrease.onclick = () => {

    store.dispatch(increase(1));

};

btnDecrease.onclick = () => {

    store.dispatch(decrease());

};

​

​

6. 구독 이란?

​

구독 subscribe도 스토어의 내장 함수 중 하나입니다. subscribe 함수 안에 리스너 함수를 파라미터로 넣어서 호출해주면, 

이 리스너 함수가 액션이 디스패치되어 상태가 업데이트 될 때마다 호출됩니다.

이 작업은 react-redux에서 알아서 해 주기 때문에 이런게 있다라고 알고 넘어가는 것이 좋습니다.

​

​

* 리덕스의 세 가지 규칙

​

1. 단일 스토어 

​

하나의 애플리케이션 안에는 오직 하나의 스토어만 있어야 한다. 여러 개의 스토어를 만들면 안 된다.

​

2. 읽기 전용 상태

​

리덕스 상태는 읽기 전용입니다. 그러므로 상태를 업데이트할 때 기존의 객체는 건드리지 않고 새로운 객체를 생성해 주어야 한다.

(spread 연산자, immer 같은 불변성 관리 라이브러리 사용)

​

​

3. 리듀서는 순수한 함수

​

변화를 일으키는 리듀서 함수는 순수한 함수여야 한다. 순수한 함수는 다음 조건들을 만족합니다.

​

1. 리듀서 함수는 이전 상태와 액션 객체를 파라미터로 받습니다.

​

2. 파라미터 외의 값에는 의존하면 안 됩니다.

​

3. 이전  상태는 절대로 건드리지 않고, 변화를  준 새로운 상태 객체를 만들어서 반환합니다.

​

4. 똑같은 파라미터로 호출된 리듀서 함수는 언제나 똑같은 결과 값을 반환해야 합니다.

​

리듀서를 작성할 때는 위 네 가지 사항을 주의해야 한다.

​

리듀서 내에서 Date 함수로 현재 시간을 가져오거나, 네트워크 요청을 하는 짓거리는 절대로 해서는 안 된다.

이러한 작업들은 리듀서 함수 바깥에서 처리해야 한다. 액션을 만드는 과정에서 해도 되고, 추후 공부할 리덕스 미들웨어에서 처리해도 된다.

주로 네트워크 요청과 같은 비동기 작업은 미들웨어를 통해 관리한다.

​

* 리덕스 코드 작성의 흐름

​

1. 액션 타입과 액션 생성 함수를 작성한다

​

2. 이어서 리듀서를 작성한다.

​

3. 스토어를 만든다.

​

4. 스토어를 구독한다. (이는 react-redux라는 라이브러리에서 자동으로 해줍니다.)

​
