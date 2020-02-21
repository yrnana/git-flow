# 1. 로컬 환경 구축

- origin : 메인 repository
  - https://github.com/KoreanNationalElection/KoreaGeneralElection.git
- local : repository를 로컬로 clone

### 1) 깃헙에서 dev 브랜치 생성
- remote : web gui에서 생성

### 2) repository를 로컬로 clone
```console
$ git clone https://github.com/KoreanNationalElection/KoreaGeneralElection.git
```
아래의 명령어를 합한 것이다
```console
$ git init
$ git remote add origin https://github.com/KoreanNationalElection/KoreaGeneralElection.git
$ git fetch origin master
```

### 3) local에 dev 브랜치 생성
```console
$ git checkout -b dev origin/dev
```

<br>

# 2. 기본 루틴

### 0) 첫 작업 전, dev 브랜치를 동기화
```console
$ git checkout dev # 현재 dev 브랜치가 아니라면 dev로 이동
$ git pull origin dev # 원격의 dev 브랜치에서 받아옴
```

### 1) 새로운 기능 개발을 위해 브랜치 생성
```console
$ git checkout -b <branch-name> dev # dev 브랜치에서 생성
```
아래의 명령어를 합한 것이다
```console
$ git branch <branch-name> dev
$ git checkout <branch-name>
```

### 2) 파일 수정
기능 개발을 위해 파일을 수정한다

### 3) 새로운 기능 브랜치를 커밋 후 원격 저장소에 push
```console
$ git commit -am "Commit Message" # 스테이징 + 커밋 동시에
$ git push -u origin <branch-name> # 기능 브랜치를 원격에 업로드
```

### 4) Pull Request 요청
기능 브랜치 -> dev 브랜치로의 merge를 요청한다

### 5) 원격 저장소와 로컬 저장소를 동기화
```console
$ git checkout dev # 현재 dev 브랜치가 아니라면 dev로 이동
$ git pull origin dev # 원격의 dev 브랜치에서 받아옴
```

### 6) 이전 브랜치 삭제
기능 추가 작업을 완료했다면 로컬에서 이전 작업 브랜치는 삭제한다
```console
$ git branch -d <branch-name>
$ git push --delete origin <branch-name> # 원격 브랜치 삭제
```

<br>

# 3. 상황별 대처
### 1) 브랜치가 아닌 dev에서 작업하고 있었다면
커밋 전이라면 새로운 브랜치를 만들면 옮겨짐
```console
$ git checkout -b <branch-name>
```

### 2) 수정 후 커밋 했는데 dev에서 작업하고 있었다면
현재 상태로 새로운 브랜치를 만들고, dev를 커밋 전의 상태로 되돌린다
```console
$ git checkout -b <branch-name>
$ git checkout dev
$ git reset --hard HEAD~1 
```

### 3) 브랜치를 만들어야 하는데, dev에 코드리뷰가 끝나지 않은 커밋이 존재함
기존 작업을 브랜치에서 하지 않고 바로 dev에 커밋하여 pr을 날린 경우임
해당 커밋이 생성되기 전의 상태로 새로운 브랜치를 만듬
```console
$ git checkout -b <branch-name> HEAD~1
```

### 4) 로컬에만 있어야 하는 파일을 push 했을 때
```console
$ git rm --cached <filename>
```

### 5) dev가 과거 버전인 상태에서 브랜치를 생성, 수정했을 때
```
$ git checkout <branch-name>
$ git add .
$ git commit -am "commit message"
$ git checkout dev
$ git pull origin dev
$ git checkout <branch-name>
$ git merge dev
```

# 4. 참고
- https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow
- https://gmlwjd9405.github.io/2017/10/27/how-to-collaborate-on-GitHub-1.html
- https://gmlwjd9405.github.io/2018/05/12/how-to-collaborate-on-GitHub-3.html
