# 1. fork한 저장소 싱크 맞추기

오픈소스나 다른 사람들의 프로젝트에 지속적으로 기여를 하다보면 `fork`한 원격저장소와 오리지날 원격저장소(오픈소스 등)의 버전차이가 생기게 된다. 단발성 PR을 하는 경우, 싱크를 맞춰 줄 필요가 없지만 지속적으로 PR을 보낼 때는 충돌을 야기하는 원인이 될 수 있으므로 싱크를 맞추는 방법을 알아보자.



## 1.1 upstream이란

먼저 upstream의 개념을 알고 넘어 갈 필요가 있다. upstream은 직역하면 "상류"를 의미하는데 높은 곳에서 내려오는 물줄기를 말한다. Git에서의 upstream 또한 비슷한 개념이다. A 저장소와 B 저장소가 존재하며 A 저장소를 B 저장소가 pull하고 push한다면, 루트가 되는 저장소는 A 저장소라고 말할 수 있다. 이는 상류와 같은 개념으로 A 저장소가 upstream이 된다.

이 관계는 흔히 fork를 통해 만들어지는데 오픈소스 저장소의 경우가 upstream, fork 저장소가 origin이 되게 된다. 해당 문서에서는 upstream과 origin의 싱크를 맞추는게 목적인 것이다.



## 1.2 싱크 맞추기

싱크를 맞추기 위해서 먼저 upstream을 원격 저장소로서 추가할 필요가 있다.

```bash
$ git remote add upstream {upstream 주소}
```



저장소를 추가한 뒤 upstream을 `fetch`해주자(`fetch`는 `pull`과 달리 자동으로 `merge`를 하지 않음).

```bash
$ git fetch upstream
...
 * [new branch]        master        -> upstream/master
 * [new branch]        sync-340ce434 -> upstream/sync-340ce434
 * [new branch]        sync-b52aa942 -> upstream/sync-b52aa942
 * [new branch]        sync-e4e6a50b -> upstream/sync-e4e6a50b
```



이후 위의 `upstream/master` 브랜치를 `merge`하면 끝이다.

```bash
$ git merge upstream/master
Merge made by the 'recursive' strategy.
...
```





