> https://dsc-sookmyung.tistory.com/28
### React에서의 불변성 유지

```
handleCreate = (data) => {
	// this.state.information.push(data); => 안됨!
   	this.setState({
    	information: this.state.information.concat(data)
    })
}
```

리액트에서 배열이나 객체를 다룰 때 **불변성을 지키는 것**이 굉장히 중요하다.

예를 들어, shouldComponentUpdate에서 데이터가 업데이트되어야 하는 상황인지 판단할 때 불변성을 지킨 경우에는 다음 받아올 데이터가 현재 데이터와 다른지 확인하기만 하면 되지만, 불변성을 지키지 않고 기존 배열 push하는 식으로 데이터를 갱신하게 되면 다음 받아올 데이터 레퍼런스와 현재 데이터 레퍼런스가 가리키는 배열이 일치하므로 비교할 수 없다.

그러므로 setState()를 이용하여 information의 값을 현재 데이터에 concat()을 통해 값을 이어붙여 만든 새로운 배열을 넣어 갱신하는 식으로 코드를 작성해야 한다.

### Id를 통해 배열 엘레먼트 다루기

배열을 랜더링할 때 각 엘레먼트마다 고유한 값인 **key**가 필요하다. 직접 값을 주지 않으면 배열 인덱스를 key값으로 사용한다. 리액트에서 내부적으로 삭제, 추가 등을 할 때 작업을 효율적으로 하기 위해서 사용한다. 

key를 사용하지 않아도 렌더링은 잘 되지만, 배열에 값이 추가되면 배열 값을 다시 불러오므로 웹이 굉장히 비효율적으로 작동할 수 있다. 삭제도 마찬가지로, 삭제된 부분만큼 뒷부분에서 값을 가져와 덮어쓰면서 배열 값을 다시 불러와야 한다.

```
id = 0;

state = {
    information: []
}
   
handleCreate = (data) => {
   const { information } = this.state;
   this.setState({
       information: information.concat({ // 불변성 유지 위해 concat 사용
           ...data,
           id: this.id++
       }),
   });
   
    /* 이렇게 하는 방법도 있다
    this.setState({
      information: information.concat(Object.assign({}, data, {
        id: this.id++
      }))
    });
    */
}
```

### 배열 내장 함수 map()

[제로초 - map, reduce 활용하기](https://www.zerocho.com/category/JavaScript/post/5acafb05f24445001b8d796d)

[

(JavaScript) map, reduce 활용하기 - 함수형 프로그래밍

안녕하세요. 이번 시간에는 map과 reduce 메서드에 대해 알아보겠습니다. 배열에 있는 바로 그 map과 reduce 맞습니다. 많은 분들이 forEach는 사용하시는데 map과 reduce는 잘 안 쓰시더라고요. 그리고 redu

www.zerocho.com



](https://www.zerocho.com/category/JavaScript/post/5acafb05f24445001b8d796d)

map은 배열의 각 원소에 대해 일괄적으로 동일한 처리를 하는 함수이다. 기존 객체를 수정하는 것이 아니라, 각 원소에 대해 우리가 정의한 함수대로 처리한 결과를 담은 새로운 객체를 만들어 낸다.

```
import React, { Component } from 'react';
import PhoneInfo from './PhoneInfo';

class PhoneInfoList extends Component {
    static defaultProps = {
        data: [],
    }
    render() {
        const { data, onRemove, onUpdate } = this.props;
        console.log('rendering list');
        const list = data.map( // map 이용
            info => (<PhoneInfo onRemove={onRemove} onUpdate={onUpdate} info={info} key={info.id}></PhoneInfo>)
            // key = 배열 랜더링할 때 필요한 고유한 값, 최적화 문제. 안쓰면 배열 인덱스를 key로 씀
        )
        return (
            <div>
                {list}
            </div>
        );
    }
}

export default PhoneInfoList;
```

 render()의 3번째 줄에서 data 배열에 map 함수를 적용하여 각 엘레먼트의 정보를 PhoneInfo 컴포넌트에 담아 새로운 배열을 만들어 list에 저장했다. 이 list 배열을 리턴하기만 하면 data 배열의 모든 정보를 PhoneInfo 컴포넌트에 담아 렌더링할 수 있다.

### 불변성을 유지하며 데이터 제거, 수정하기

#### 데이터 제거 - .slice()나 .filter() 사용

slice() 메서드는 인덱스를 통해 배열의 부분 배열을 반환한다. 기존 배열의 값을 수정하는 것이 아니라 불변성을 유지하며 데이터를 제거할 수 있다. slice() 메서드는 2개의 인자를 필요로 하는데, 첫번째 인자는 시작 인덱스고 두번째 인자는 끝 인덱스이다. 범위는 반열림구간이다( \[x, y) ).

```
const numbers = [1, 2, 3, 4, 5];
const newNumbers = numbers.slice(0, 2);
// newNumbers = [1, 2]
```

filter() 메서드는 특정 조건을 가지고 값을 필터링할 수 있다. 삭제하려는 인덱스와 비교하기만 하면 되므로 slice()보다 더 쉽게 데이터를 삭제할 수 있다. slice()와 마찬가지로 기존 배열의 값을 갱신하는 것이 아니라 새로운 배열을 생성하므로 불변성을 유지할 수 있다.

```
const numbers = [1, 2, 3, 4, 5];
const newNumbers1 = numbers.filter(n => n > 3);
// newNumbers1 = [4, 5];
const newNumbers2 = numbers.filter(n => n !== 3);
// newNumbers2 = [1, 2, 4, 5];
```

리액트에서 filter() 함수를 통해 삭제 기능을 구현한 코드이다.

```
  handleRemove = (id) => {
    const { information } = this.state;
    this.setState({
      information: information.filter(info => info.id !== id)
    });
  }
```

#### 데이터 수정 - .slice()나 .map() 사용

slice() 메서드와 [비구조화 할당](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)을 통해 데이터를 수정할 수 있다.

```
const numbers = [1, 2, 3, 4, 5];
[
    ...numbers.slice(0, 2),
    9
    ...numbers.slice(3, 5)
]
// > [1, 2, 9, 4, 5]
```

map() 메서드를 통해서도 데이터를 수정할 수 있는데, 지금 인덱스가 수정하려는 인덱스와 같은지 비교하여 참일 때만 값을 수정하도록 하면 된다. 

```
  handleUpdate = (id, data) => {
    const { information } = this.state;
    this.setState({
      information: information.map(
        info => {
          if (info.id === id) { // 수정하려는 인덱스인가?
            return {
              id,
              ...data, // 갱신하려는 데이터 넣기
            };
          }
          return info; // 수정하지 않는 경우
        }
      )
    });
  }
```
