## chroot

- change root
- 프로세스는 실제 root 경로가 아니라 설정한 경로를 root처럼 인식

### 실습
- debootstrap
    - Ubuntu 내에서 Debian 계열의 OS를 guestOS로 설치
    - Hypervisor가 없으며, chroot 기술을 이용
    - 호스트 커널이 게스트 커널을 관리할 수 있음
    - 명령어
        ```bash
        sudo debootstrap <suite> <target> <mirror>
        ```
    - 예제
        - suite : 운영 체제 세트, 게스트 OS로 사용할 운영 체제를 지정
        - target 폴더 생성 후 mirror 지정
        ```bash
        sudo debootstrap xenial /mnt/chroot-ubuntu-xenial/ http://archive.ubuntu.com/ubuntu/
        ``` 
- chroot
    - 새 터미널을 열어서 chroot 실행
    - chroot는 마운트 지점과 bash를 연결
    - 예제
        ```bash
        sudo chroot /mnt/chroot-ubuntu-xenial/ /bin/bash
        ```
- 확인
    ```bash
    cat /etc/os-release
    ```