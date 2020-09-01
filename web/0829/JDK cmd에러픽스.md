# 1. JDK cmd 에러픽스

JDK설치 후 JRE는 `java` 명령어로 바로 사용가능하지만, JDK의 경우 시스템 환경변수를 설정해주어야한다. 만약 설정 이후에도 다음과 같이 에러가 발생한다면 해당 문서의 방법을 사용해보자.

```bash
C:\Users\11>javac -version
'javac'은(는) 내부 또는 외부 명령, 실행할 수 있는 프로그램, 또는
배치 파일이 아닙니다.

C:\Users\11>set path=%path%;%JAVA_HOME%\bin

C:\Users\11>javac -version
javac 14.0.2
```



위와 같이 정상적으로 적용이 된다면 `setx` 명령어로 영구적으로 설정을 해주자.

```bash
C:\Users\11>setx path "%path%;%JAVA_HOME%\bin"
```

