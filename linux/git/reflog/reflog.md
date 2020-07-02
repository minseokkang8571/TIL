# 1. reflog

Git의 `rebase`나 `reset` 명령어를 사용해 commit을 되돌렸을 때, 이를 다시 되돌리고 싶은 경우가 있다. 하지만 `log`에는 이후의 해쉬가 남아있지 않기 때문에 이를 되돌릴 수 없는 이런 상황을 위해 `reflog`라는 명령어가 존재한다.



## 1.1 commit 되돌리기

`reflog`의 가장 큰 용도는 앞서 언급한 commit의 HEAD를 원위치하는 것이다.

다음과 같은 log를 가지고 있는 상황에서의 예를 보자.

```bash
$ git log --oneline
1c8a581 (HEAD -> master) README | bracket grammar revised
10ab913 (origin/master) web 0629 | Dango ORM finished
cd28121 README | TIL rule added and Dango ORM updated
b66174a web 0625 | content chagned
...
```



위와 같은 상황에서 우리는 `git reset --hard cd28121` 명령어를 통해 'TIL rule added...'라는 이름의 커밋으로 HEAD를 변경할 수 있다. 이 때 log를 확인하면 다음과 같다.

```bash
$ git log --oneline
cd28121 (HEAD -> master) README | TIL rule added and Dango ORM updated
b66174a web 0625 | content chagned
4117341 web 0625 | image path changed
8785a07 README | cors updated
...
```



그런데 막상 commit을 되돌렸더니, 이전의 commit으로 돌아가야겠다는 생각이 든다. 하지만 `log`를 통해 해당 커밋을 볼 수 없으니 이같은 명령을 할 수 없는 상황이 되어버렸다. 만약 `reset`이나 `rebase`를 하며, 앞선 commit들의 데이터가 사라졌다면 이를 되돌릴 수 없겠지만 실제로 Git에서는 HEAD의 위치가 변해 log가 보이지 않을 뿐 이를 저장하고 있다.

다음과 같은 명령어를 사용해보자.

```bash
$ git reflog
cd28121 (HEAD -> master) HEAD@{0}: reset: moving to cd28121
1c8a581 HEAD@{1}: commit: README | bracket grammar revised
...
```



`reflog`를 사용하면 명령어를 사용함에 따른 HEAD의 위치를 알 수 있을 뿐만 아니라 어떤 명령어를 사용했었는지까지 알 수 있다. 기록을 보면 우리는 **1c8a581**에 위치에서 `reset`명령어를 통해 **cd28121**로 HEAD를 변경했음을 알 수있다.

그럼 이전의 commit으로 되돌아가보자. 현재 상황에서 방법은 두가지이다.

```bash
$ git reset --hard 1c8a581 # 1. 이전의 해쉬값을 사용한다.
$ git reset --hard HEAD@{1} # 2. 되돌리고 싶은 타임라인의 HEAD@값을 사용한다.

HEAD is now at 1c8a581 README | bracket grammar revised
```





