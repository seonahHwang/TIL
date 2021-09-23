# Merge & Rebase

# Merge

1. fast-forward merge

    동일 내용이 포함되는 브랜치일 경우 브랜치 이동만으로 병합해서 따로 commit을 생성하지 않는 경우 

2. 3-way merge는 변경사항을 합쳐서 새로운 커밋을 만들어낸다 
    => 두개의 부모를 가리키는 특별한 커밋을 만드는 것. 두 부모의 모든 부모들의 작업내역 포함  

# Rebase

브랜치의 공통 조상이 되는 base를 다른 브랜치의 커밋 지점으로 바꾸는 것 

```bash
git checkout feature
git rebase master
git checkout master
git merge feature 
```

위의 merge는 fast-forward merge

비슷한 결과를 만드는 다른 방식으로, `C3` 에서 변경된 사항을 Patch로 만들고 이를 다시 `C4` 에 적용시키는 방법이 있다. Git에서는 이런 방식을 ***Rebase*** 라고 한다. `rebase` 명령으로 한 브랜치에서 변경된 사항을 다른 브랜치에 적용할 수 있다.

위의 예제는 아래와 같은 명령으로 Rebase 한다.

`$ git checkout experiment
$ git rebase master 
First, rewinding head to replay your work on top of it...
Applying: added staged command`

실제로 일어나는 일을 설명하자면 일단 두 브랜치가 나뉘기 전인 공통 커밋으로 이동하고 나서 그 커밋부터 지금 Checkout 한 브랜치가 가리키는 커밋까지 diff를 차례로 만들어 어딘가에 임시로 저장해 놓는다. Rebase 할 브랜치(역주 - experiment)가 합칠 브랜치(역주 - master)가 가리키는 커밋을 가리키게 하고 아까 저장해 놓았던 변경사항을 차례대로 적용한다.

![https://git-scm.com/book/en/v2/images/basic-rebase-3.png](https://git-scm.com/book/en/v2/images/basic-rebase-3.png)

Figure 37. `C4`의 변경사항을 `C3`에 적용하는 Rebase 과정

그리고 나서 `master` 브랜치를 Fast-forward 시킨다.

`$ git checkout master
$ git merge experiment`

![https://git-scm.com/book/en/v2/images/basic-rebase-4.png](https://git-scm.com/book/en/v2/images/basic-rebase-4.png)

참고 

[🎢 Git Rebase 활용하기](https://velog.io/@godori/Git-Rebase)

[[Git] branch 병합하기 (merge), fast forward와 merge commit의 차이점](https://yuja-kong.tistory.com/51)

[Git - Rebase 하기](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0)
