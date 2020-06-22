# 1. stash

git으로 branch를 이용해 작업을 하다 팀원의 요청이나 필요에 의해 먼저 처리해야 할 일이 생길 경우, commit이 이루어진 이후에 branch를 이동해야 하기 때문에 의도치 않은 commit을 해야한다. 우리는 `stash`라는 명령어를 통해 이 문제를 해결 할 수 있다.

stash 명령을 사용하면 워킹 디렉토리내에 변화된 내용을 보관하고 워킹 트리를 비울 수 있다. 즉, branch이동에 제약이 사라지게된다. 예를 들어 branch 이동시에는 다음과 같이 사용할 수 있다. 

branch A (변경내용 있음) **->** stash **->** branch A (변경내용 없음) **->** branch B 이슈 처리 후 branch A로 이동 **->** stash pop



### git stash

해당 명령어를 사용하면 새로운 stash를 스택에 만들고 변경사항을 워킹 디렉토리에서 비우게 된다.

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   test.txt

$ git stash
warning: LF will be replaced by CRLF in test.txt.
The file will have its original line endings in your working directory
Saved working directory and index state WIP on master: d26340a test added

$ git status
On branch master
nothing to commit, working tree clean
```



`Untracked files`의 경우 stash에 포함되지 않는데 이를 포함하기 위해서는 옵션을 추가해주어야 한다. 이렇게 추가된 stash는 인덱스로 관리되는데 stash는 **스택**에 쌓이는 것이므로 새롭게 추가되는 stash가 0번 인덱스를 받게 된다.

```bash
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        test.txt

$ git stash --include-untracked
Saved working directory and index state WIP on master: 4efc6fc first commit

$ git status
On branch master
nothing to commit, working tree clean
```



### git stash list

stash 스택을 확인 할 수 있다. stash는 리스트에서 볼 수 있는 `stash@{0}`과 같은 인덱스로 관리되게 된다.

```bash
$ git stash list
stash@{0}: WIP on master: 4efc6fc first commit
```



### git stash apply

스택에 있는 stash의 내용을 적용 할 수 있다. 명령어를 보면 `stash@{[index]}`와 같이 인덱스로 접근하는 것을 볼 수 있는데 이를 생략할 경우 스택의 맨 앞에서 꺼내오게 된다.

```
$ git stash apply stash@{0}
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   stash.md

no changes added to commit (use "git add" and/or "git commit -a")
```



### git stash drop

스택에 있는 stash를 내용을 적용하지 않고 제거한다. stash를 apply하고 난 후에도 스택에는 여전히 남아있게 되므로 해당 명령어를 사용하여 관리해야 한다. 만약 apply와 drop을 한번에 사용하고 싶은 경우 `git stash pop`명령어를 사용하면 된다.

```bash
$ git stash drop
Dropped refs/stash@{0} (3b1fb5ff7aac078d92538cacf83919b7c33cfe36)
```

