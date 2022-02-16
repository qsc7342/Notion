#  **:pencil: Github를 공부하며 학습한 내용들**
<hr/>

    팀 프로젝트등을 진행할 때, github를 항상 사용해왔지만,
    commit과 pull, push 등을 제외하고는 거의 사용할 줄 모르고
    branch 등을 효율적으로 사용하지 못해 main에서만 진행하며, 
    merge 충돌을 해결하지 못해 git clone을 남발하는 경우가 다반사였다. 
    이렇게 계속 무의미하게 github를 사용하는 것은 
    발전이 없을 것 같아 본격적으로 공부해보기로 하였다.
- - -
* ## Git에 파일 및 폴더 배제  
  + 포함하면 안되는 정보를 담은 파일  
  
  + 라이브러리나 DS_Store 등과 같은 파일
  
  + **.gitignore 파일을 사용하여 해결한다.**

        최상위 폴더 파일 배제: /filename
        모든 확장자명 파일: *.확장자
        폴더와 하위 요소: 폴더명/
        미포함: !파일명

        더 자세한 내용은 
        https://git-scm.com/docs/gitignore를 참고하자.
- - -


* ## Git 커밋 되돌리기 
  + reset은 해당 시점 이후의 커밋 내역을 삭제한다.

  + revert는 해당 시점으로 되돌아가되, 그 이후의 커밋 내역은 유지한다.  
  
    
        git log (깃 커밋 내역 조회)
        git reset --hard 해쉬값 (해쉬값 미포함시 가장 최근 커밋으로)
        git revert 해쉬값
        git revert --no-commit 해쉬값 (revert 하지만, 커밋 미진행)
- - -

* ## Git 브랜치  
  <br/>
* ### 브랜치 기본
  + 여러 기능들을 독립적으로 개발할 때 사용한다.  
 
        git branch (브랜치 조회 및 현재 위치 확인)
        git branch 브랜치명 (브랜치 생성)
        git switch 브랜치명 (브랜치 이동)
        # 이전에는 checkout을 사용하였지만, 2.23 이후 switch, restore로 분리되었다.

        git switch -c 브랜치명 (생성과 이동 동시에 진행)
        git branch -d 브랜치명 (브랜치 삭제)
        git branch -m 브랜치명 새이름 (브랜치 이름 변경)

        git log --all --decorate --oneline --graph (모든 브랜치 로그 보기)

  <br/>

  ### 브랜치 병합
  + 두 브랜치의 내용을 합친다.

  + 크게 merge와 rebase 방식 존재

  + merge는 브랜치 사용 기록이 남으며,  
    rebase는 브랜치를 메인 브랜치에 이어붙인다.
  
  + 협업 시에, merge가 선호된다.(pull 제외)

        merge는 병합하려는 브랜치로 이동 후, 합칠 브랜치를 선언한다.

        git merge 브랜치명

        rebase는 작업한 브랜치로 이동 후 진행한다.

        git rebase 병합하려는 브랜치명
        git merge 작업브랜치명

        rebase를 진행하면 작업한 브랜치가 앞에 존재하므로, merge를 추가로 진행한다.

  + merge 진행 시, 충돌이 발생할 수 있다.  
    만약 충돌 처리가 힘들다면 아래를 통해 취소하자.

        git merge --abort
        git rebase --abort
  + 충돌을 해결한 뒤에 git commit -am으로 병합이 완료된다.  
  rebase의 경우에는 여러 번 커밋될 수 있으므로,

        git rebase --continue

    를 통해 끝까지 rebase를 진행한다.
- - -
* ## 원격 저장소(remote)  
  <br/>
    *  원격 저장소 생성


      git remote add 원격저장소명 레포주소
    
    대부분 원격저장소명으로는 origin을 사용한다.  

      
    <br/>
    *  pull과 push

      git pull --no-rebase (merge 방식)
      git pull --rebase (rebase 방식)
      git push --force (강제 푸쉬)


    <br/>
    *  원격 브랜치

      git branch -all (모든 브랜치 조회)
      git branch는 로컬 브랜치만 조회한다.

      git push -u origin 브랜치명 (로컬 브랜치를 원격 저장소에 푸시)
      
      git fetch (원격 브랜치 받아오기)
      git switch -t 원격브랜치명 (로컬에 원격브랜치 연동)
      git push origin --delete 브랜치명 (원격 브랜치 삭제)

      git fetch와 git pull은 커밋을 로컬로 가져온 뒤,
      merge를 하느냐의 차이에 있다.
- - -
* ## 공간 제어 
  + git은 Working Dir, Staging Area(add), Repository(commit)  
  세 가지 공간으로 구분할 수 있다.

    
        git restore --staged 파일명 
        (staging area에서 work dir로 이동)

        git reset --option
        --soft : staging area
        --mixed : working dir로 이동 (default)
        --hard : 수정사항 삭제

  + HEAD는 현재 속한 브랜치의 가장 최신 커밋을 의미
    + checkout을 통해 이동한다.
    + ^ or ~를 붙이면 갯수만큼 이전으로 이동한다.

          git checkout HEAD^^
          HEAD 전전 위치로 이동

          git checkout -
          이동 취소 (한 단계 되돌림)
- - -
* ## 편의성
  + help

        git help
        git help -a 모든 명령어 조회
        git commit -h commit에 대한 help

  + config

        git config (global) --list
        git config (global) key value

        전역 및 지역 조회 및 설정

        git config --global core.autocrlf true
        git config --global core.autocrlf input

        윈도우의 경우 true, MAC input
        줄바꿈 호환 문제를 해결해주는 설정이다.

        git config pull.rebase false
        git config pull.rebase true

        pull의 설정을 rebase로 할 지, merge로 할 지

        git config --global init.defaultBranch main
        
        기본 브랜치명을 main으로 설정

        git config --global alias.(단축키) commit
        커밋을 (단축키)로 사용할 수 있게끔함
- - -
* ## 메세지 컨벤션
  + feat: 기능 추가
  + fix: 버그 수정
  + docs: 문서 수정
  + style: 공백, 세미콜론 등 스타일 수정
  + refactor: 리팩토링
  + perf: 성능 개선
  + test: 테스트 추가 (TDD에서 쓰나보다)
  + chore: 빌드 과정 또는 보조 과정 수정
  + **특정 이슈와 관련된 커밋 메세지의 경우 Closes #Num 등을 Footer에 달자**
- - -