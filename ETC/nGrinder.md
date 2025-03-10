## nGrinder

- 네이버에서 제공하는 오픈소스 서버 부하 테스트 툴
- [깃헙 페이지](https://github.com/naver/ngrinder?tab=readme-ov-file)

### 구성

- Controller : 테스트 관리, 스크립트 배포, 테스트 결과 수집 및 분석
- Agent : 테스트를 실제로 수행하는 노드, 여러개의 에이전트를 활용해 부하를 분산 시킬 수 있음

### 설치

- 실행 파일을 다운받거나 Docker 를 활용하여 설치 가능

#### 실행파일 다운

1. 컨트롤러 다운로드 [깃헙 릴리즈](https://github.com/naver/ngrinder/releases)
2. 컨트롤러 실행
```shell
  java -jar ngrinder-controller-3.5.9-p1.war --port=8050
```
3. 실행시킨 localhost:8300 으로 접속
- id : admin
- pw : admin
4. 우측 상단 메뉴에서 Agent Download 
5. 에이전트 압축 해제 후 bat or sh 실행
#### 도커 사용
1. Controller 설치
```shell
docker pull ngrinder/controller
docker run -d -v ~/ngrinder-controller:/opt/ngrinder-controller --name controller \
-p 8050:80 -p 16001:16001 -p 12000-12009:12000-12009 ngrinder/controller
```
2. Agent 설치
```shell
docker pull ngrinder/agent
docker run -d -v ~/ngrinder-agent:/opt/ngrinder-agent --name agent ngrinder/agent controller_ip:controller_web_port
```