# Git

[📌commit 취소](#commit-취소)  
[📌새로 만든 저장소에 기존 저장소 내용과 history 모두 복제하기](#새로-만든-저장소에-기존-저장소-내용과-history-모두-복제하기)  
[📌git branch 이름 변경](#git-branch-이름-변경)  
[📌git remote branch 삭제](#git-remote-branch-삭제)  
[📌staging 됐던 파일 다시 untrackted 하기](#📌staging-됐던-파일-다시-untrackted-하기  )  



## 📌commit 취소

[방법 1] commit을 취소하고 해당 파일들은 staged 상태로 워킹 디렉터리에 보존     

```
$ git reset --soft HEAD^
```

[방법 2] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에 보존
```$ git reset --mixed HEAD^ // 기본 옵션
$ git reset HEAD^ // 위와 동일
$ git reset HEAD~2 // 마지막 2개의 commit을 취소
```


[방법 3] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에서 삭제   
```
$ git reset --hard HEAD^
```

<br>


## 📌새로 만든 저장소에 기존 저장소 내용과 history 모두 복제하기

기존 저장소 https://A

새로운 저장소 https://B

인 경우

    $ cd A
    $ git clone git clone --mirror https://A
    $ git remote set-url --push origin https://B
    $ git push --mirror

위와 같이하면 새로운 저장소에 기존저장소 코드와 히스토리가 모두 복제된다

출처: https://effectivecode.tistory.com/868 [Mr.후]  

<br>


## 📌git branch 이름 변경  
```
$ git branch -m 변경전_branch_name 새로운_branch_name
```  
<br>



## 📌git remote branch 삭제  
```
$ git push [remote-name] --delete [old-branch-name]
```  

## 📌staging 됐던 파일 다시 untrackted 하기  

git rm --cashed [파일/폴더명]  
