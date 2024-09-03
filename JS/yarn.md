## Yarn
- Node.js의 패키지 관리 툴
- React 같은 프로젝트를 진행하며 겪은 어려움을 해결하기 위해 개발됨
- npm 과 같은 기능을 수행하나 npm 레지스트리와 호환하며 속도나 안정성 측면에서 npm 보다 향상됨

### 설치
- 설치
```shell
npm install --global yarn
```
- 버전 확인
```shell
yarn -v
```

### 명령어
- yarn init : package.json 생성
- yarn or yarn install : package.json 파일 및 해당 종속성에 나열된 모든 모듈 설치
- yarn add package_name @버전 : 특정 패키지의 특정 버전 설치
- yarn global add package_name : 옵션, 글로벌로 설치, 로컬의 다른 프로젝트도 이 패키지를 사용 가능하게 됨
- yarn remove : 패키지 삭제 명령어
- yarn upgrade : 설치한 패키지들을 업데이트
- npm dedupe : 중복 설치된 패키지들을 정리

### npm vs yarn
#### 속도
- npm은 패키지를 한번에 하나씩 순차적으로 설치
- yarn은 여러 패키지를 동시에 가져오고 설치하도록 최적화
- 현재도 yarn이 빠르긴 하나 속도 면에서 크게 앞서지는 않아 큰 차이를 보이진 않음

#### 보안
- yarn이 보안 측면에서 npm보다 더 안전한것으로 알려짐
- npm은 자동으로 패키지에 포함된 다른 패키지 코드를 실행하여 편리하나 안정성에 위협이 될 수 있음
- yarn은 yarn.lock or package.json 파일에 있는 파일만 설치
- 현재는 npm도 보안이 향상된 편