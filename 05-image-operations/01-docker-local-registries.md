## Docker Local Registries
- Docker Hub를 통하지 않고, 도커 엔진 내 이미지 레지스트리 생성
- registry라는 컨테이너를 띄워서 사용

### 자료
- [레지스트리 여러 개 생성](https://distribution.github.io/distribution/about/deploying/#customize-the-published-port)
- registry:2 : 태그 2의 레지스트리 이미지
- HTTP 주소 수정이 필요함 `-e REGISTRY_HTTP_ADDR=0.0.0.0:5001`
- 기본 포트는 5000