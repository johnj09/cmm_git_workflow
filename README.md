# Git Workflow

## 브랜치
- **main**: 배포하는 브랜치
    - 배포 가능한 상태만을 관리한다
- **development**: 기능 개발을 위한 브랜치들을 merge 하기 위한 브랜치
    - 모든 기능이 추가 되고 버그가 수정되어 배포 가능한 안정적이 상태면 main 브랜치에 merge 한다
- **feature**: 세부 기능을 구현하고 직접적인 개발을 하는 브랜치
    - development 브랜치에서 분기하여 기능 개발 및 버그 수정을 한다
    - 개발이 완료되면 development 브랜치에 merge 한다

## Getting Started
본인이 개발을 진행할 <u>empty</u> working directory를 만들고 아래의 커맨드를 터미널에 입력하여 local git repository를 initialise 한다.
```
git init
```
아래의 커맨드를 입력하여 local git repository가 initialise 됐는지 확인한다.
```
git status
```
Local git repository 셋업이 완료됐다면 개발중인 GitHub Repository를 remote repository로 추가하고 데이터를 fetch 해온다.
```
git remote add origin https://github.com/url/to/repository
git fetch origin
```
이 단계에선 아직 local directory에는 아직 아무 파일도 없지만, remote branch가 추가 돼있는걸 확인 할 수 있다.
```
git branch -a
```
개발을 시작 할 수 있도록 feature branch(Local)를 development branch(Remote)에서 만든다.
```
git checkout -b feature/feature_name remotes/origin/development
```
- 주의 사항:
    - feature_name은 자신이 맡은 feature에 연관되지만 너무 길지 않게 (최대 2단어)로 만들자
    - 커맨드의 마지막 argument는 다를 수도 있는데 아래- 커맨드를 입력하고 나오는 remote development branch 이름을 사용하자
    ```
    git branch -a
    ```

이제 local directory 안에 파일이 생성돼있는걸 볼 수 있다.

## Start Developing
**이제부터 개발을 시작하면 된다.**

개발을 했다면 먼저 변경된 파일들을 확인하고 본인이 추가 및 수정한 부분을 add 한다.
```
git status

git add .                       // 변경점이 있는 모든 파일을 add 한다.
or
git add "./path/to/file"        // 변경점이 있는 파일 "./path/to/file" 을 add 한다.
```
- 주의 사항:
    - ```git add .``` 커맨드는 root directory 에서 사용하자

변경점을 add 했다면 코드를 commit 하면 된다.

```
git commit                                      // VS code 를 쓴다면 파일 하나가 열리는데, 여기에 commit message 를 기입하면 된다.
or
git commit -m "This is a test message"          // "This is a test message" 라는 메세지와 함께 commit 한다
```

- 주의 사항:
    - Commit message 를 여러 줄 쓸때 첫번째 줄은 Title, 그 후의 줄들은 Body가 되니 염두에 두고 메세지를 쓰자

Commit을 했다면 계속 개발을 하거나 다음 단계로 넘어간다.

Feature의 일부분이 완성돼서 GitHub Repo에 올리고 싶다면 변경점들을 push 하면 된다.

```
git push -u origin feature/feature_name
```
- 주의 사항:
    - main 브랜치나 development 브랜치에는 절대로 직접적으로 push 하지 않도록 하자

위의 커맨드가 성공적으로 실행되면 GitHub Repo에
- 처음 push 했을 때
    - feature/feature_name 브랜치가 새로 본인의 마지막 commit과 동일하게 생성돼 있는걸 볼 수 있다
- Push한 적이 있으며 GitHub에 이미 feature/feature_name 브랜치가 있을 때
    - GitHub의 branch가 업데이트된걸 볼 수 있다

만약 Push가 실패 했다면 내가 Pull 했던 시점과 Push하려는 시점 사이에 누군가가 Push하려는 브랜치에 이미 Push했을 가능성이 크다.<br>
그럴 때에는 한 번 더 Pull 해서 누군가가 Push한 변경점을 현재 내 코드와 merge하고 다시 Push하면 된다.

이제 위의 단계들을 Feature가 완성될 때까지 반복한다.

## Pull Requests
이제 Feature가 모두 완성됐고 이 Feature를 development 브랜치에 implement하면 된다.

우선 github.com으로 가서 현재 일하고 있는 Repository로 가고 브랜치를 본인의 feature로 변경한다.<br>
그러면 화면에 Compare & pull request 옵션이 뜰것이다. (안 뜬다면 상단의 navigation bar에서 pull requests로 가고 new pull request 옵션을 선택한다.)

여기서 base 브랜치와 compare 브랜치를 선택 할 수 있는데 base 브랜치는 **변경점을 push하는 브랜치**, compare 브랜치는 **변경한 브랜치**이므로 주의 깊게 선택 한다.
- 주의 사항:
    - 위에도 얘기했듯이 **절대로** main 브랜치에는 push하거나 pull request를 열지 말자

Base 브랜치와 Compare 브랜치를 선택했다면 conflict가 없을 경우엔 바로 pull request를 열 수 있지만, conflict가 있다면 먼저 conflict를 resolve하고 pull request를 열어야 한다.

이제 두 브랜치 사이에 conflict가 없다면 pull request를 열면 된다.<br>
Pull request를 열 때는:
- Title은 이 feature가 무엇인지 짧고 간략하게 설명
- Description은 정확히 어떤 변경점들이 있는지, 다른 개발자들이 알아야 할 내용들을 추가

이제 각 팀의 팀장과 PM은 이 Pull Requests들을 리뷰하며 문제점이 없고 development 브랜치에 merge 해도 된다고 판단 될때 pull request를 approve 한다.
- 이 단계에서 코드 리뷰를 해도 좋다

개발이 끝난 Feature 브랜치는 지워도 좋다.
