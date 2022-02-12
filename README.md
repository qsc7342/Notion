#  **:pencil: Github를 공부하며 학습한 내용들**
<hr/>

    팀 프로젝트등을 진행할 때, github를 항상 사용해왔지만, commit과 pull, push 등을 제외하고는 거의 사용할 줄 모르고 merge 충돌을 해결하지 못해 git clone을 남발하는 경우가 다반사였다. 이렇게 계속 무의미하게 github를 사용하는 것은 발전이 없을 것 같아 본격적으로  공부해보기로 하였다.
<br/>

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
- - -

