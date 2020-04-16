# git bash 환경에서 wget추가하기

git bash 환경에서 다양한 명령어를 추가해 사용 할 수 있다. 해당 문서에서는 wget을 예를 들어 설명한다.

- 먼저, wget파일을 다운로드 받는다 (zip등의 형태로)
- `wget64.exe`or `wget.exe`를 `Git/mingw64/bin/`폴더로 옮긴다
- 파일의 이름을 `wget.exe`으로 변경한다.



위의 과정을 거치면 다음과 같이 `wget`을 명령어로 사용 할 수 있다.

```bash
$ wget
wget: missing URL
Usage: wget [OPTION]... [URL]...
```

