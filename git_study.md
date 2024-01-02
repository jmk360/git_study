# git study

## 1. Git 설치
- Windows 환경인 경우
    - Git 공식 사이트: https://git-scm.com/ 여기에서 Git을 설치한다.
    - Git을 설치하는 과정에서 "Git Bash Here"와 "Git GUI Here" 체크가 있는데, 나는 Git GUI는 사용하지 않기 때문에, "Git GUI Here"는 체크하지 않고 Git 설치를 진행하면 된다.
- Linux 환경인 경우
    ```bash
    sudo apt-get update
    sudo apt-get install git
    ```

## 2. Git 설치 확인 방법
터미널 환경에서 "git --version"을 입력하여 git version이 정상적으로 출력이 되면, git이 정상적으로 설치가 되어있는 거다.
만약 git version이 출력이 되지 않는다면, git이 설치가 되어 있지 않은 것 이다.

## 3. Git 최초 설정을 해야 하는 것들

> git config --global 옵션의 경우 전역으로 git 설정을 한다는 의미이다.
> 만약 특정 프로젝트에서만 git config를 설정하려고 한다면, 해당 프로젝트로 이동하여 --global 옵션을 주지 않고, git config를 설정하면 된다.

- git 최초 설정
    ```bash
    # user의 이름을 설정한다.
    git config --global user.name <name>

    # user의 이메일을 설정한다.
    git config --global user.email <email>

    # Windows, Linux(Mac) 환경에서 개발자들이 협업하는 경우 개행(엔터)처리 방식때문에 발생하는 이슈를 방지한다.
    # <?>에는 Windows인 경우  true, Linux(Mac)인 경우 input으로 설정한다.
    git config --global core.autocrlf <?>

    # Git의 기본 branch를 main으로 바꾼다. 아무것도 설정하지 않으면, master branch가 기본 branch이다.
    git config --global init.defaultBranch main

    # Pull을 받을때는 2가지 정책이 있는데, 첫번째는 merge방식, 두 번째는 rebase방식이다. pull.rebase를 false로 설정하면, pull 받을때 merge 방식으로 pull을 받게되고, pull.rebase가 true이면, pull받을때, rebase방식으로 pull을 받게 된다. 나는 pull 받을때 merge방식을 채택하기 때문에, pull.rebase를 false로 설정한다.
    git config --global pull.rebase false

    #Push를 할 때, 별도의 옵션 없이, 자동으로 현재의 branch명과 동일한 원격의 branch에게 push를 한다.
    git config --global push.default current

    # 원격저장소의 ID, PW를 저장하여 ID, PW를 입력하지 않아도 된다.
    # 단, 나 혼자 사용하는 pc에서만 사용해야한다. 협업하는 pc에서 사용할 경우, 해킹 당할 수 있다.
    # Windows인 경우 "Windows 자격 증명" 에 등록하여 ID, PW를 저장할 수 있다.
    # Linux인 경우 개인 PC에서 아래 명령어로 ID, PW를 저장하여 사용하자
    git config --global credential.helper store

    # >>>>>>>>>>>>>>>>>>>> 기본 editor 변경하는 설정 추가하기

    # git alias 등록
    git config --global alias.gl "log --all --decorate --graph --oneline"
    git config --global alias.glf "log --graph --all --pretty=format:'%C(yellow) %h %C(reset)%C(blue)%ad%C(reset) : %C(white)%s %C(bold green)-- %an%C(reset) %C(bold red)%d%C(reset)'"
    ```
- git 설정 확인하는 방법

    1. 일일이 확인
    ```bash
    git config --global user.name
    git config --global user.email
    git config --global core.autocrlf
    git config --global init.defaultBranch
    git config --global pull.rebase
    git config --global push.default
    git config --global credential.helper
    git config --global alias.gl
    git config --global alias.glf
    ```
    2. git config를 editer를 통해서 확인 및 수정, 삭제가 가능하다.
    ```bash
    git config --global --edit
    ```
    또는
    ```bash
    git config --global -e
    ```
    3. .gitconfig 파일을 통해 확인 및 수정, 삭제가 가능하다.
    - Windows인 경우: 사용자 계정 폴더 하위의 .gitconfig 파일을 확인한다.
    - Linux인 경우: $HOME 폴더 하위의 .gitconfig 파일을 확인한다.

- 터미널에서 git branch이름 나타내기
    - Windows인 경우 git bash 터미널을 사용하면 git branch이름을 자동으로 나타내준다.
    - Linux인 경우 아래와 같이 ~/.bashrc에 추가한다.
        ```bash
        # git show Branch name
        parse_git_branch() {
            git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
        }
        export PS1="\u@\h \[\033[32m\]\w\[\033[33m\]\$(parse_git_branch)\[\033[00m\] $ "
        ```

## 4. git config --global core.autocrlf 설정에 대해서

※ 참고사이트: https://git-scm.com/book/ko/v2/Git%EB%A7%9E%EC%B6%A4-Git-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0

> 협업할 때 겪는 소스 포맷(Formatting)과 공백 문제는 미묘하고 난해하다. 동료 사이에 사용하는 플랫폼이 다를 때는 특히 더 심하다. 다른 사람이 보내온 Patch는 공백 문자 패턴이 미묘하게 다를 확률이 높다. 편집기가 몰래 공백문자를 추가해 버릴 수도 있고 크로스-플랫폼 프로젝트에서 Windows 개발자가 라인 끝에 CR(Carriage-Return) 문자를 추가해 버렸을 수도 있다. Git에는 이 이슈를 돕는 몇 가지 설정이 있다.

```core.autocrlf```
>Windows에서 개발하는 동료와 함께 일하면 라인 바꿈(New Line) 문자에 문제가 생긴다. Windows는 라인 바꿈 문자로 CR(Carriage-Return)과 LF(Line Feed) 문자를 둘 다 사용하지만, Mac과 Linux는 LF 문자만 사용한다. 아무것도 아닌 것 같지만, 크로스 플랫폼 프로젝트에서는 꽤 성가신 문제다. Windows에서 사용하는 많은 편집기가 자동으로 LF 스타일의 라인 바꿈 스타일을 CRLF로 바꾸거나 Enter 키를 입력하면 CRLF 스타일을 사용하기 때문이다.

>Git은 커밋할 때 자동으로 CRLF를 LF로 변환해주고 반대로 Checkout 할 때 LF를 CRLF로 변환해 주는 기능이 있다. core.autocrlf 설정으로 이 기능을 켤 수 있다. Windows에서 이 값을 true로 설정하면 Checkout 할 때 LF 문자가 CRLF 문자로 변환된다.

>$ git config --global core.autocrlf true

>라인 바꿈 문자로 LF를 사용하는 Linux와 Mac에서는 Checkout 할 때 Git이 LF를 CRLF로 변환할 필요가 없다. 게다가 우연히 CRLF가 들어간 파일이 저장소에 들어 있어도 Git이 알아서 고쳐주면 좋을 것이다. core.autocrlf 값을 input으로 설정하면 커밋할 때만 CRLF를 LF로 변환한다.

>$ git config --global core.autocrlf input

>이 설정을 이용하면 Windows에서는 CRLF를 사용하고 Mac, Linux, 저장소에서는 LF를 사용할 수 있다.

>Windows 플랫폼에서만 개발하면 이 기능이 필요 없다. 이 옵션을 false 라고 설정하면 이 기능이 꺼지고 CR 문자도 저장소에도 저장된다.

>$ git config --global core.autocrlf false

## 5. git config --global credential.helper 설정에 대해서

※ 참고사이트: https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Credential-%EC%A0%80%EC%9E%A5%EC%86%8C

## 6. Windows 자격 증명 등록하는 방법

## 7. 프로젝트 생성 & Git 관리 시작
git에서 관리하고자 하는 프로젝트 폴더 하위에서 아래 명령어를 수행한다.
```bash
git init
```
위 명령어를 수행하면, 프로젝트 폴더 하위에 .git(local repository)가 생성된다.
.git 폴더를 삭제하면, git 관리내역이 삭제된다.(현 파일들은 유지)
최신 커밋상태에서 뭔가 변화가 생기면, 아래 명령어를 통해서 변화 상황을 확인할 수 있다.
```bash
git status
```
- 추적하지 않는(untracked) 파일: git의 관리에 들어간 적 없는 파일

    -> 현재 커밋 기준으로 새로 생성된 파일은 git에서 관리 된적이 없기에 untracked에서 보여진다.

## 8. .gitignore

git의 관리에서 특정 파일/폴더를 배제해야 할 경우에 사용한다.
- 포함할 필요가 없을 때: 자동으로 생성 또는 다운로드되는 파일들(빌드 결과물, 라이브러리)
- 포함하지 말아야 할 때: 보안상 민감한 정보를 담은 파일
- .gitignore 파일을 사용해서 배제할 요소들을 지정할 수 있다.

.gitignore 파일 형식
```bash
# 이렇게 #를 사용해서 주석

# 모든 file.c
file.c

# 최상위 폴더의 file.c
/file.c

# 모든 .c 확장자 파일
*.c

# .c 확장자지만 무시하지 않을 파일
!not_ignore_this.c

# logs란 이름의 파일 또는 폴더와 그 내용들
logs

# logs란 이름의 폴더와 그 내용들
logs/

# logs 폴더 바로 안의 debug.log와 .c 파일들
logs/debug.log
logs/*.c

# logs 폴더 바로 안, 또는 그 안의 다른 폴더(들) 안의 debug.log
logs/**/debug.log
```

## 9. commit 하는 방법

staging 하기(working directory에서 staging area로 옮기기)

```bash
# working directory에 있는 test.yaml파일을 staging area로 이동시킨다.
# 선택적으로 staging 할 수 있다.
git add test.yaml
```

```bash
# working directory의 모든 것을 staging area로 이동시킨다.
git add .
```

commit 하기

```bash
git commit
```

git editor 설정을 따로 하지 않았다면 기본으로 vi로 진입을 할 것이다. vi 화면에서 커밋 메시지를 입력한 뒤 저장하고 종료하면 commit이 된다.

※ vi 명령어

| 작업 | vi 명령어 | 상세 
---|---|---
입력 시작 | i | 명령어 입력 모드에서 텍스트 입력 모드로 전환
입력 종료 | ESC | 텍스트 입력 모드에서 명령어 입력 모드로 전환
저장 없이 종료 | :q |
저장 없이 강제 종료 | :q! | 입력한 것이 있을 때 사용
저장하고 종료 | :wq | 입력한 것이 있을 때 사용
위로 스크롤 | k | git log 등에서 내역이 길 때 사용
아래로 스크롤 | j | git log 등에서 내역이 길 때 사용

커밋 메시지까지 함께 작성하기
```bash
git commit -m "First commit"
```

커밋 내역 확인하기
```bash
git log
```