# 1. vscode에서 django 환경 구축하기

- vscode에서 기본 터미널을 `git bash`로 설정

  > vscode에서 `ctrl`+`shift`+`p`입력 후 `Terminal: Select default shell`->`bash`

- django를 `pip`를 통해 설치

  ```bash
  $ pip install django=="{원하는 버전}"
  ```

- 프로젝트 생성

  ```bash
  $ django-admin startproject {프로젝트명}
  ```

- 프로젝트 폴더로 이동 후, vscode 재실행

  ```bash
  $ cd {프로젝트명}
  $ code .
  ```

- `.gitignore`파일 생성

  ```bash
  $ touch .gitignore
  ```

- [gitignore.io](https://www.gitignore.io/) 방문 후 ignore설정

  - `python`, `visualCode`, `django` 등의 개발환경을 검색
  - 내용을 `.gitignore`에 저장
  
- local 환경에서는 기본적으로 루프백이 사용되므로, 루프백의 경우 allowed_host로 지정할 필요가 없으며, 포트 또한 8000번이 기본값으로 사용된다.

  ```bash
  $ python manage.py runserver
  Performing system checks...
  # ... #
  Starting development server at http://127.0.0.1:8000/
  ```

  