# 푸시한 커밋 author 수정하기

이미 push한 내용을 바꾸는 것은 그렇게 바람직하지 않은 것을 명심!

1.  `$ git log`를 통해 수정할 hash를 찾음
2.  `$ git rebase -i {해쉬값}`
3.  vim에디터가 열리면 변경할 커밋의 `pick`부분을 `edit`으로 수정
4.  `$ git commit --amend --author="{사용자명} <{이메일}>"`
5.  vim에디터가 열리면 commit의 이름을 수정
6.  `$ git rebase --continue`
7.  `$ git push -f {원격저장소} master`