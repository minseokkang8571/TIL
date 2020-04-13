# 1. docker

Docker는 Linux기반 Container Runtime 오픈소스로 Virtual Machine (이하 VM)보다 가벼운 형태로 배포 할 수 있는 것이 특징이다. VM의 경우 Host OS가 깔리고, 그 위에 Hypervisor 그리고 Virtual Machine이 만들어진다. 때문에 배포를 받는 경우 Host OS를 전부 받아야한다. 하지만 Container의 경우, Kernel등의 OS를 포함하지 않은채로 배포를 하는데 Kernel의 경우 Host OS를 그대로 사용하며 Host OS와 Container OS의 다른 부분만 Container안에 담아 배포 하게 된다. 이러한 특징 때문에 Container에서 명령어를 수행하면 Host OS에서 명령어가 수행된다.



## 1.1 docker 설치

```bash
git clone https://github.com/dotcloud/docker.git
```



## 1. 2 vagrant

Vagrant는 가상머신을 쉽게 Provisioning 할 수 있게 하는 툴로 생성과 관리시 사용한다. 가상머신의 Host name, IP, Service Install 등의 환경설정을 사용자의 요구에 맞게 할 수 있다.

- Vagrant를 사용하지 않는 경우 - 다수의 가상머신을 일일이 설정해주어야 함
- Vagrant를 사용하는 경우 - 가상머신에 대한 설정과 작업을 미리 정한 후 VirtualBox를 통해 Provisioning

### Vagrant 명령어

- vagrant init - vagrantfile을 생성
- vagrant up - provisioning 실행
- vagrant halt - 가상머신 종료
- vagrant destoy - 가상머신 삭제
- vagrant ssh - ssh로 접속
- vagrant provision - 가상머신의 설정을 변경

