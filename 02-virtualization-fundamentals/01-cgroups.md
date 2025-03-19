## CGroups

- Control Groups. 프로세스를 묶음으로 처리

### CGroups 조회
- `cgroup-tools` 설치 후 `lscgoup`으로 리스트 조회
- systemd 기반에서는 `systemd-cgls`로 트리로 조회 가능

### 프로세스의 cgroup 확인
```bash
sudo cat /proc/<PID>/cgroup
```

- 사용 예시
```bash
sudo cat /proc/1/cgroup
```

- 출력 결과
```bash
0::/init.scope
```

### 프로세스 제어
- 이슈 : Ubuntu 24.04 LTS에서는 freezer가 바로 노출되어 있지 않음
    - 해결 : 직접 만들기

    ```bash
    sudo mkdir /sys/fs/cgroup/freezer
    sudo mount -t cgroup -ofreezer freezer /sys/fs/cgroup/freezer
    ```

    - cgroup 마운트 위치 `/sys/fs/cgroup`
    - `-t` type. filesystem 종류 중 cgroup이 있음
    - [cgroup 마운트 옵션에서 메모리, 프리저 등 선택 가능](https://docs.kernel.org/admin-guide/cgroup-v1/cgroups.html#how-do-i-use-cgroups)

- 실습 내용
    - freezer 내 파일 확인
        ```bash
        mooji@ubuntu-cloud:/sys/fs/cgroup/freezer/mycgroup$ ls -al
        total 0
        drwxr-xr-x 2 root root 0 Mar 19 06:08 .
        dr-xr-xr-x 3 root root 0 Mar 19 06:08 ..
        -rw-r--r-- 1 root root 0 Mar 19 06:08 cgroup.clone_children
        -rw-r--r-- 1 root root 0 Mar 19 06:08 cgroup.procs
        -r--r--r-- 1 root root 0 Mar 19 06:08 freezer.parent_freezing
        -r--r--r-- 1 root root 0 Mar 19 06:08 freezer.self_freezing
        -rw-r--r-- 1 root root 0 Mar 19 06:30 freezer.state
        -rw-r--r-- 1 root root 0 Mar 19 06:08 notify_on_release
        -rw-r--r-- 1 root root 0 Mar 19 06:29 tasks
        ```
    - freezer 내 mycgroup 폴더를 만들면 위와 같은 파일이 자동으로 생성
    - `tasks`에 원하는 pid 삽입
    - `freezer.state`에 `FROZEN`/ `THAWED` 를 넣으면 해당 프로세스가 멈추거나 다시 동작하게 함
        ```bash
        sudo bash -c 'echo FROZEN > freezer.state'
        ```
    - THAWED = RESUME
    - `>` : 덮어쓰기, `>>` : 추가