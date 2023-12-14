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

> git config --global 옵션의 경우 전역으로 git 설정을 한다는 의미이다.
> 만약 특정 프로젝트에서만 git config를 설정하려고 한다면, 해당 프로젝트로 이동하여 --global 옵션을 주지 않고, git config를 설정하면 된다.

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

## 5. git config --global credential.helper 설정에 대해서

## 6. 프로젝트 생성 & Git 관리 시작
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