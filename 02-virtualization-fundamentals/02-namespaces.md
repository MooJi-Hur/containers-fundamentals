## Namespaces

### 네임스페이스 조회
- 특정 프로세스의 네임스페이스 조회
    ```bash
    sudo ls -l proc/<PID>/ns/
    ```

### 네임스페이스를 이용한 네트워크 가상화
- 네임스페이스 두 개를 만든 뒤, virtual ethernet tunnel을 연결
- `ip netns help` 네트워크 네임스페이스 관련 명령어 도움말
- `ip link help` 네트워크 인터페이스 설정 관련 도움말
    - `add` : veth 두 개를 만들어서 연결
    - `set` : 각 veth를 각 네임스페이스와 연결
    - `exec`+ `set dev .. up` : 기기에 네트워크 인터페이스 활성화 (Bring up)
    - `exec`+ `addr add` : ip 주소 할당
    - `exec`+ `ping` : 연결 결과 테스트
