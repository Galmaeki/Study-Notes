## Named Export
```js
import App from './App';
import {BrowserRouter as Router} from "react-router-dom";
```
- JS의 Export 는 위 두가지 방식으로 가능
- 중괄호로 묶어서 사용하는것을 Named Exports, 중괄호가 없는 쪽을 DefaultExports 라고 함

### Default Exports & Named Exports
#### Default Exports
- 하나의 함수, 객체, 변수를 기본값으로 내보낼 때 사용
```js
const Verification = () => {
...
}
export default default Verification
```
- 위 코드를 import 할 경우 아래처럼 사용 가능
- 아무 이름으로나 import 가능
```js
import veri from '../VerificationPage'
```
- var, let, const 로 선언된 변수는 내보낼 수 없음

##### Named Exports
- 한 파일 내에서 여러 변수/클래스 등을 export 할 수 있음
```js
export function calculateSum(a, b) {
  return a + b;
}

export function calculateDifference(a, b) {
  return a - b;
}
```
- 위 코드를 사용하려면 중괄호를 이용하여 선언 가능
- as 로 이름을 바꾸어 사용 가능
- * as 로 사용하면 한 파일에 있는 클래스/변수들을 한번에 import 가능
```js
import { calculateSum as Sum, calculateDifference } from './utils';
```

#### 정리
- 한 파일에서 여러 함수를 내보낼 때는 named, 한 개만 내보낼 때는 default 로 사용하는 편이나 팀 스타일에 따라 사용하는걸 권장