# 1. 로컬 환경 구축

- upstream : 메인 (master, dev 브랜치)
  - https://github.com/KoreanNationalElection/KoreaGeneralElection.git
- origin : 메인을 fork한 repository
  - https://github.com/YuriNaDev/KoreaGeneralElection.git
- local : fork repository를 로컬로 clone

### 1) fork repository를 로컬로 clone
```console
$ git clone https://github.com/YuriNaDev/KoreaGeneralElection.git
```

### 2) 메인 repository 등록
```console
$ git remote add upstream https://github.com/KoreanNationalElection/KoreaGeneralElection.git
```

<br>

# 2. 기본 루틴

### 1) upstream에서 최신 소스 받기 (pr 이후에도 실행)
rebase 사용시 커밋 히스토리 관리에 좋음
> master 브랜치에서 받아와야 한다
```console
$ git checkout master # 현재 master 브랜치가 아니라면
$ git pull --rebase upstream dev
$ git push origin master
```

### 2) 새로운 브랜치 생성하기
dev 브랜치에서 개발 이슈에 해당하는 feature 브랜치를 만들어 작업  
브랜치 이름은 feature/기능 이런식으로 통일하면 좋다
> master 브랜치에서 브랜치를 생성해야 한다!
```console
$ git checkout master # 현재 master 브랜치가 아니라면
$ git checkout -b <new-branch-name> # <new-branch-name> 브랜치로 바뀜
```
두번째 줄은 이 두 명령어를 합한 것이다
```console
$ git branch <new-branch-name>
$ git checkout <new-branch-name>
```

### 3) 수정한 파일 추가 & 커밋
```console
git add .
git commit -m "commit message"
```

### 4) 커밋을 fork repository로 올리기
```console
$ git push origin <new-branch-name>
```

### 5) 메인 repository로 pull request 날리기
fork repository에서 메인 repository로 코드리뷰를 위한 pr 날리기

### 6) pull request merge 후 브랜치 정리
```console
$ git checkout master # 현재 master 브랜치가 아니라면
$ git pull --rebase upstream dev # 최신 소스로 로컬 업데이트
```
```console
$ git branch -d <new-branch-name> # delete branch
$ git push origin master # update fork/master
$ git push --delete origin <new-branch-name>
```

<br>

# 3. 발생 가능한 상황

### 1) 브랜치가 아닌 dev에서 작업하고 있었다면?
아직 커밋을 하지 않은 상태이므로, 그냥 새로운 브랜치를 만들면 옮겨짐
```console
$ git checkout -b <new-branch-name>
```

### 2) 수정 후 커밋 했는데 dev에서 작업하고 있었다면?
현재 상태로 새로운 브랜치를 만들고, dev를 커밋하기 전의 상태로 되돌린다
```console
$ git checkout -b <new-branch-name>
$ git checkout dev
$ git reset --hard HEAD~1
```

### 3) 브랜치를 만들어야 하는데, dev에 코드 리뷰가 끝나지 않은 커밋이 있다
기존 작업을 브랜치에서 하지 않고 바로 dev에 커밋하여 pr을 날렸을 경우임  
해당 커밋이 생성되기 전의 상태로 새로운 브랜치를 만든다
```console
$ git checkout -b <new-branch-name> HEAD~1
```

### 4) 뭔가 이상하다면 reset
```console
$ git reflog
$ git reset --hard HEAD@{1} # 돌아가기 원하는 위치로
```

### 5) 메인 repository에 push 후 이상해진걸 깨달았다
이미 public으로 나간 커밋은 revert 하고 push
```console
$ git log
$ git revert <commit-hash>
```

### 6) merge 커밋을 revert 하고 싶다면
```console
$ git revert -m 1 <commit-hash>
```

### 7) 로컬에만 있어야 하는 파일을 push했을때
```console
$ git rm --cached <filename>
```

### 8) 브랜치 이름을 바꾸고 싶을 때
```console
$ git branch -m <old-name> <new-name>
```

### 9) 비슷한 커밋을 정리
```console
$ git log --oneline
$ git rebase -i HEAD~3 # interactive rebase
```
pick을 s로 변경

<br>

# 4. 참고
- https://meetup.toast.com/posts/116
- https://andamiro25.tistory.com/193
- https://blog.scottlowe.org/2015/01/27/using-fork-branch-git-workflow/