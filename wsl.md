# WSL
## 설정
* [ms](https://docs.microsoft.com/ko-kr/windows/wsl/wsl2-install)
* 보고 하면 그대로 된다.
## docker
* [docker](https://docs.docker.com/engine/install/ubuntu/)
* 역시 보고 하면 된다.
## mariadb
* [docker hub](https://hub.docker.com/_/mariadb)
* wsl에서 도커 설치 후 
```sh
docker pull maraidb
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=1234 --name mariadb mariadb
```
* 하면 끝 
* wsl은 자체 ip를 가지므로 wsl환경에서 
```sh
ip addr show eth0
```
* 위 명령어를 통해 wsl의 ip 주소를 알 수 있고 이 ip주소에 docker 실행 시 설정한 포트를 활용하면 접속가능하다.
