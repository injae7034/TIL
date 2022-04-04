# Git 작업 가이드

1. 시작하기<br>
새 저장소 만들고 시작하기<br>
원격 저장소를 가져와서 시작하기<br>
Sourcetree에 저장소 폴더 추가하기<br>
2. 원격 저장소 추가하기<br>
원격 저장소 목록 확인<br>
원격 저장소 정보 자세히 보기<br>
origin 원격 저장소 추가<br>
upstream 원격 저장소 추가<br>
3. 작업 하기<br>
Step \#1 - 작업 브랜치 만들기<br>
Step \#2 - upstream 원격 저장소의 최신 상태를 반영하기<br>
Step \#3 - 작업하기<br>
Step \#4 - 커밋<br>
Step \#5 - origin 원격 저장소에 작업 브랜치 올리기<br>
Step \#6 - Pull Request<br>
4. 작업 수정하기<br>
Step \#1 - 해당 브랜치로 이동하기<br>
Step \#2 - 수정하기<br>
Step \#3 - 커밋<br>
Step \#4 - origin 원격 저장소에 작업 브랜치 올리기<br>
Step \#5 - Pull Request가 바뀐 것 확인하기<br>
5. Merge된 브랜치 정리하기<br>
Step \#1 - 내 컴퓨터의 main 브랜치에 최신 코드 반영하기<br>
Step \#2 - Merge된 브랜치 확인<br>
Step \#3 - 내 컴퓨터의 브랜치 삭제<br>
Step \#4 - 원격 저장소의 브랜치 삭제<br>
6. 차력사를 위한 레시피<br>
Pro Git 읽기<br>
clone 기능 쓰지 않고 비슷하게 따라하기<br>
pull --rebase 기능 쓰지 않고 비슷하게 따라하기<br>
다른 브랜치의 특정 커밋 가져오기<br>
예시 상황: 실수로 upstream/main을 쓰지 않고 브랜치를 만들었다<br>





## 시작하기
### 새 저장소 만들고 시작하기

git init<br>


### 원격 저장소를 가져와서 시작하기

git clone <원격 저장소 주소><br>


### Sourcetree에 저장소 폴더 추가하기

Sourcetree를 실행해 방금 만들거나 가져온 폴더를 추가합니다.<br>
뭔가를 할 때마다 Sourcetree를 계속 보시면 Git에 대해 알 수 있고, 무슨 일이 벌어지고 있는지도 알 수 있습니다.<br>

## 원격 저장소 추가하기

### 원격 저장소 목록 확인

git remote<br>


### 원격 저장소 정보 자세히 보기

git remote -v<br>


### origin 원격 저장소 추가

\# clone으로 원격 저장소를 가져오면 origin 원격 저장소가 이미 추가된 상태입니다.<br><br>

git remote add origin \<내 원격 저장소 주소\><br><br>

git fetch origin<br><br>


### upstream 원격 저장소 추가

git remote add upstream \<공용 원격 저장소 주소\><br><br>

git fetch upstream<br><br>


## 작업 하기

git status<br><br>

### Step \#1 - 작업 브랜치 만들기

git switch -c \<브랜치 이름\> upstream/main<br><br>

\# upstream/main은 붙여서 쓰고, 가운데 슬래시(/)가 들어갑니다.<br><br>

### Step \#2 - upstream 원격 저장소의 최신 상태를 반영하기

git pull --rebase upstream main<br><br>

\# upstream과 main 사이엔 공백이 들어갑니다.<br><br>

### Step \#3 - 작업하기

원하는 작업을 이 시점에 합니다.<br><br>


### Step \#4 - 커밋

\# 뭔가 바뀐 점을 추가합니다.<br><br>
\# 새 파일을 추가하는 게 아니라, 파일 추가/변경/삭제란 “바뀐 점”을 추가합니다.<br><br>

git add .<br><br>

\# 방금 추가한 바뀐 점을 커밋합니다.<br><br>
\# 가능하면 메시지를 우리가 나중에 찾아볼 수 있는 형태로 씁니다.<br><br>

git commit<br><br>

### Step \#5 - origin 원격 저장소에 작업 브랜치 올리기

git push origin \<브랜치 이름\><br><br>

\# origin과 \<브랜치 이름\> 사이엔 공백이 들어갑니다.<br><br>

### Step \#6 - Pull Request

GitHub에서 New Pull Request를 합니다.<br><br>

## 작업 수정하기

Pull Request를 했지만 수정하고 싶을 때가 있습니다. 코드 리뷰 내용을 보고 반영하기 위해 고칠 때가 많은데, 해당 브랜치에 가서 Commit을 추가하고 Push만 하면 됩니다.<br><br>

### Step \#1 - 해당 브랜치로 이동하기

git switch \<브랜치 이름\><br><br>

### Step \#2 - 수정하기

변경 작업을 이 시점에 합니다.<br><br>


### Step \#3 - 커밋

\# 뭔가 바뀐 점을 추가합니다.<br><br>
\# 새 파일을 추가하는 게 아니라, 파일 추가/변경/삭제란 “바뀐 점”을 추가합니다.<br><br>

git add .<br><br>

\# 방금 추가한 바뀐 점을 커밋합니다.<br><br>
\# 가능하면 메시지를 우리가 나중에 찾아볼 수 있는 형태로 씁니다.<br><br>

git commit<br><br>

### Step \#4 - origin 원격 저장소에 작업 브랜치 올리기

git push origin \<브랜치 이름\><br><br>

### Step \#5 - Pull Request가 바뀐 것 확인하기

뭔가 새로운 작업을 하지 않아도 Pull Request가 바뀐 것을 확인할 수 있습니다.<br><br>


## Merge된 브랜치 정리하기

### Step \#1 - 내 컴퓨터의 main 브랜치에 최신 코드 반영하기

\# main 브랜치로 이동<br><br>

git switch main<br><br>

\# 최신 코드 반영<br><br>

git pull --rebase upstream main<br><br>

\# origin 원격 저장소에 main 브랜치를 올려보기 (필수는 아니지만 기분이 좋아짐)<br><br>

git push origin main<br><br>


### Step \#2 - Merge된 브랜치 확인

\# 내 컴퓨터에 있는 브랜치 중 Merge된 것 보기<br><br>
\# 이 목록에 main도 포함된다는 점에 주의!<br><br>

git branch --merge<br><br>

\# 내 컴퓨터와 원격 저장소에 있는 브랜치 중 Merge된 것 보기<br><br>
\# 이 목록에 main도 포함된다는 점에 주의!<br><br>

git branch -a --merge<br><br>


### Step \#3 - 내 컴퓨터의 브랜치 삭제

\# 안전하게 삭제<br><br>

git branch -d \<브랜치 이름\><br><br>

\# 강제로 삭제<br><br>

git branch -D \<브랜치 이름\><br><br>

### Step \#4 - 원격 저장소의 브랜치 삭제

git push origin :\<브랜치 이름\><br><br>


## 차력사를 위한 레시피

### Pro Git 읽기

https://git-scm.com/book/ko/v2<br><br>

매우 압축적으로 써진 좋은 책입니다. 꽤 어렵지만 여기 있는 걸 모두 연습하는 게 좋습니다.<br><br>

분량이 많지 않아 보이지만, 다른 책의 몇 배의 내용을 담고 있습니다. 이 책은 Git 명령 몇 개를 전달하는 걸 목표로 하는 게 아니라,<br><br>
Git 자체를 이해하는 걸 목표로 하고 있기 때문이죠.<br><br>
따라서 이 책을 참고해서 여러 실험을 해보시는 게 중요합니다.<br><br>

특히 10장을 읽는 건 매우 크게 도움이 됩니다.<br><br>

### clone 기능 쓰지 않고 비슷하게 따라하기

\# 작업 폴더를 만들고 이동합니다.<br><br>

mkdir \<프로젝트 이름\><br><br>
cd \<프로젝트 이름\><br><br>

\# 내 컴퓨터에 저장소를 만듭니다.<br><br>

git init<br><br>

\# 브랜치와 원격 저장소가 모두 없음을 확인합니다.<br><br>

git branch<br><br>
git remote<br><br>

\# 설정도 특별한 게 없습니다.<br><br>

cat .git/config<br><br>

\# origin 원격 저장소를 추가합니다.<br><br>

git remote add origin \<원격 저장소 주소\><br><br>

\# 원격 저장소가 추가된 걸 확인합니다.<br><br>

git remote<br><br>

git remote -v<br><br>

\# 설정에 remote가 추가된 걸 확인합니다.<br><br>

cat .git/config<br><br>

\# 아직 브랜치가 없습니다.<br><br>

git branch<br><br>

git branch -a<br><br>

\# origin 원격 저장소의 내용을 가져옵니다.<br><br>

git fetch origin<br><br>

\# 내 컴퓨터엔 브랜치가 없지만, 원격 저장소의 브랜치를 알게 된 걸 확인합니다.<br><br>

git branch<br><br>

git branch -a<br><br>

\# origin 원격 저장소의 main 브랜치를 이용해 내 컴퓨터에 main 브랜치를 만듭니다.<br><br>

git switch -c main origin/main<br><br>

\# 내 컴퓨터에 브랜치가 생긴 걸 확인합니다.<br><br>

git branch<br><br>

\# clone은 원격 저장소의 모든 브랜치를 내 컴퓨터로 가져오지만, 여기서는 main만 가져옵니다.<br><br>
\# 저는 이게 훨씬 깔끔해서 이걸 좋아합니다.<br><br>

### pull --rebase 기능 쓰지 않고 비슷하게 따라하기

원격 저장소 upstream의 main을 활용하고 있는 상황을 가정하겠습니다.<br><br>

\# 내 컴퓨터에 있는 원격 저장소 upstream의 사본이 어떤 commit을 main으로 쓰고 있는지 확인합니다.<br><br>
\# 만약 fetch를 한번도 한 적이 없다면 여기서 에러가 납니다. → 괜찮습니다. 다음으로 넘어가시면 됩니다.<br><br>

cat .git/refs/remotes/upstream/main<br><br>

\# 내 컴퓨터에 있는 원격 저장소 upstream의 사본을 최신으로 업데이트합니다.<br><br>

git fetch upstream<br><br>

\# 내 컴퓨터에 있는 원격 저장소 upstream의 사본이 최신으로 업데이트된 걸 확인합니다.<br><br>
\# 만약 위에서 확인한 것과 변화가 없다면 이미 최신인 상황입니다. → 괜찮습니다. 좋은 겁니다.<br><br>

cat .git/refs/remotes/upstream/main<br><br>

\# 작업 브랜치로 이동합니다. 이미 해당 브랜치를 쓰는 상태라면 안 해도 됩니다.<br><br>

git switch \<브랜치 이름\><br><br>

\# 작업 브랜치를 원격 저장소 upstream의 main 브랜치를 참고해 리베이스(rebase, 재배치)합니다.<br><br>

git rebase upstream/main<br><br>

\# Sourcetree 등 시각화 도구를 이용해 내 브랜치가 올바른 곳에 위치하고 있는지 확인합니다.<br><br>

### 다른 브랜치의 특정 커밋 가져오기

내 브랜치에 커밋이 너무 많고 작업이 꼬인 것 같으면 새 브랜치를 만들어서 특정 커밋만 가져오는 게 도움이 됩니다.<br><br>

### 예시 상황: 실수로 upstream/main을 쓰지 않고 브랜치를 만들었다

\# 브랜치 1과 브랜치 2를 따로 만들고 싶었고, 그래서 브랜치 1을 만듭니다.<br><br>

git switch -c test-20190813 upstream/main<br><br>

\# 뭔가 작업 후 커밋.<br><br>

echo "# 2019년 8월 13일" \> ahastudio/20190813.md<br><br>

git add .<br><br>

git commit -m "2019년 8월 13일 문서 추가"<br><br>

\# 다른 브랜치를 만들 때 실수로 upstream/main을 빼먹음.<br><br>

git switch -c test-20190814<br><br>

\# 뭔가 작업 후 커밋.<br><br>

echo "# 2019년 8월 14일" \> ahastudio/20190814.md<br><br>

git add .<br><br>

git commit -m "2019년 8월 14일 문서 추가"<br><br>

\# Sourcetree를 보니 뭔가 잘못 됐음을 발견.<br><br>
\# upstream/main을 붙여서 새로운 브랜치 2를 만든다.<br><br>

git switch -c new-test-20190814 upstream/main<br><br>

\# 아까 실수한 브랜치 2의 가장 마지막 커밋만 가져온다.<br><br>
\# 만약 특정 커밋을 가져오고 싶다면 브랜치 이름 대신 커밋 ID를 쓰면 됩니다.<br><br>
\# 브랜치는 사실 특정 ID의 별명(alias)에 불과합니다.<br><br>

git cherry-pick test-20190814<br><br>

\# Sourcetree를 보고 원하는 모양으로 바뀐 걸 확인한다.<br><br>
\# 이제 아까 실수한 브랜치 2를 강제로 삭제한다.<br><br>

git branch -D test-20190814<br><br>

\# 새 브랜치 2의 이름을 바꿔서 아무 일도 없는 것처럼 위장한다.<br><br>

git branch -m new-test-20190814 test-20190814<br><br>

\# Sourcetree를 보고 흡족함을 느낀다.<br><br>
\# 기존에 원격 저장소로 push한 적이 있다면 -f 플래그 옵션을 붙여서 강제로 push해야 한다.<br><br>

git push origin test-20190814 -f<br><br>
