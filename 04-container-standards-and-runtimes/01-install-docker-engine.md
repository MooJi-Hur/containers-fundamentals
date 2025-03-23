## Install Docker Engine
- docker 저장소를 통해 설치 및 서비스 상태 확인

### Docker 저장소 연결
- HTTPS 저장소 설정에 필요한 패키지 설치
    ```bash
    sudo apt install -y apt-transport-https ca-certificates curl gnupg lsb-release
    ```
    - apt-transport-https : HTTPS 프로토콜로 저장소에 접근
    - ca-certificates : 인증서 제공 패키지
    - curl : URL로 데이터 다운로드 및 전송
    - gnupg : GNU Privacy Guard 암호화 및 서명, 키 검증
    - lsb-release : 리눅스 배포판 정보 출력, 배포판 코드명 확인

- Docker GPG 키를 저장
    ```bash
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    ```
    - curl 옵션
        - -f : HTTP 오류 발생 시 실패
        - -s : 진행 상황 출력 생략
        - -S : 오류 발생 시 에러 메세지 출력
        - -L : url 리다이렉트 반영
    - gpg 옵션
        - --dearmor : ASCII 형식의 GPG 키를 바이너리로 변환
        - -o 변환 결과를 지정한 파일에 출력
- 저장소 추가
    ```bash
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```
    - $(lsb_release -cs) 우분투 배포판 코드명
    - tee는 파일 입력과 화면 출력을 함께 수행
        - echo 명령어와 함께 `>` 리다이렉션도 일반 사용자 권한으로 실행됨
        - sudo tee 명령어를 사용하여 표준 입력을 루트 권한으로 파일에 씀
        - /dev/null로 표준 출력을 화면에 보이지 않도록 함

### Docker 데몬 관리 초기 설정

- systemd를 통해 데몬 서비스 관리
    - 데몬 상태 확인
        ```bash
        sudo systemctl status docker.service
        ```
    - systemctl은 systemd를 관리하는 도구

- 일반 사용자로 Docker를 관리하기 위한 설정
    - docker 그룹 생성, 현재 UID를 docker group에 추가
        ```bash
        sudo usermod -aG docker $USER
        newgrp docker
        ```
        - -aG : append to group
        - newgrp docker : 현재 세션에서 그룹 변경을 바로 반영