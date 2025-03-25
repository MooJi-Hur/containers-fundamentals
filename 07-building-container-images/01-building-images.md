## Building Docker Images
- Dockerfile로 이미지 만들기

### 과정
- docker client에서 docker build 명령어를 받음, 
- build process를 실행
    - Dockerfile이 있는 context 폴더를 찾음
    - 특정 Dockerfile 위치를 원할 경우 -f 옵션 사용
    - context는 tarball(압축)로 저장 후 docker daemon으로 보내짐
- docker host의 daemon
    - 이미지에 이름과 태그를 붙임
    - 각 빌드 프로세스 실행 (레이어 쌓기)


### 동일한 이름의 이미지 생성 시 후처리
```bash
docker image ls --quiet --filter=dangling=true |
xargs --no-run-if-empty sudo docker image rm
```
- 동일한 이름으로 이미지를 생성하면 기존 이미지는 이름과 태그가 \<none>으로 변경됨 (dangling)
- 해당 이미지 아이디 목록을 받아 삭제
