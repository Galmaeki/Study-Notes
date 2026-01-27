## React-router-DOM
- React 앱의 라우팅 처리를 위한 라이브러리
- React 의 React-Router 라이브러리 기반으로 만들어짐

#### 장점
- 설치와 설정이 쉬움
  - npm or yarn을 통해 간단한 설치
- 선언적 라우팅
  - 컴포넌트 기반 라우팅 정의 가능
  - 코드의 가독성 및 유지보수성을 높임
- 다양한 라우팅 기능
  - 링크, 중첩된 라우팅, 동적 경로 등 복잡한 라우팅 처리 가능

### 설치
1. React-Router-dom 라이브러리 설치
   - ```shell
      yarn add react-router-dom
     ```
   - 타입 스크립트를 사용한다면 아래도 추가
   - ```shell
      yarn add --dev @types/react-router-dom
     ```
2. BrowserRouter 로 App 컴포넌트를 감싸 라우팅 활성화
3. Route 로 경로와 해당 경로에 대응하는 컴포넌트 정의
4. Link 로 다른 경로로 이동하는 링크 생성