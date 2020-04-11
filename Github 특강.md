# Git

> Git은 분산버전관리 시스템(DVCS) 중 하나이다. 

수업 내용은 [git bash](https://gitforwindows.org/) 를 기반으로 설명한다. 



## Git기초흐름

### 0. 저장소 설정

```bash
$ git init Initialized empty Git repository in C:/da/.git/ $
```



* git 저장소를 만들게 되면 해당 디렉토리 내에 .git/ 폴더가 생성된다 
* git `bash` 에서는 (`master`)라는 표기가 같이 등장한다. 
  * 현재 작업중인 브랜치를 의미한다

### 1. `add` 명령어 

* 해당 폴더에서 작업 후 상황 

```bash
# a.txt를 만든 상황
$ git status
On branch master

No commits yet
# 트래킹 되고 있지 않은 파일들
# => git으로 버전을 남긴적이 없는 파일
Untracked files:
  # staging area에 포함시키려면, git add
  # (커밋될 파일 목록)
  (use "git add <file>..." to include in what will be committed)
        a.txt
# 커밋할 내용 없지만, 트래킹 되지 않는 파일은 존재한다.
# => WD O, Staging Area X
nothing added to commit but untracked files present (use "git add" to track)

```

* staging area에 추가 

  ```bash
  $ git add a.txt        #a.txt파일 
  $ git add myfolder/    #myfolder 폴더
  $ git add .            #현재 디렉토리
  ```

* 추가 후 상태 

  ```bash
  $ git status
  on branch master 
  
  No commits yet
  # 커밋될 변경사항들 (staging area)
  Changes to be committed:
    (use "git rm --cached <file>..." to unstage)
         # a.txt 새로 생성됨
          new file:   a.txt
  ```


### 2. `commit` 명령어 

> 커밋 메시지는 현재 버전에 대해 명확하게 작성

```bash
$ git commit -m "커밋메시지"
[master (root-commit) 41a723a] Init
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 a.txt
```

* 커밋 이력을 확인하기 위해서는 아래의 명령어를 입력한다.

  ```bash
  $ git log
  commit 41a723a3626a9737a7eb822441f4ba0aede37d9d (HEAD -> master)
  Author: edutak <edutak.ssafy@gmail.com>
  Date:   Fri Apr 10 10:45:22 2020 +0900
  
    Init
  $ git log 				
  $ git log -1			#최근 한 개 커밋 
  $ git log --oneline		#간략한 로그
  $ git log --oneline -1	#최근 한 개의 커밋을 간략하게 
  ```
  



## Git 상태 메시지

 ```bash
$ git status
# branch 정보
On branch master
Your branch is up to date with 'origin/master'.

# 커밋될 변경사항
# Staging area
Changes not staged for commit:
	# WD => staging area
	(use "git add <file>..." to update what will be committed)
	# WD 작업 내용을 모두 삭제(되돌릴 수 없음)
	(use "git restore <file>..." to discard changes in working directory)
		modified: a.txt
 ```



### 0. 원격 저장소 설정 

* 원격 저장소 설정 

  ```bash
  $ git remote add origin {__url__}
  ```

  * 원격저장소(`remote`)로 `origin` 이름으로 `url`을 추가(`add`)

* 원격 저장소 목록 

  ```bash
  $ git remote -v
  origin https://github.com/edutak/test-repo.git (fetch
  origin https://github.com/edutak/test-repo.git (push)
  
  ```

* 원격 저장소 삭제 

  ```bash
  $ git remote rm origin
  ```

  * `origin` 이름의 원격 저장소 설정을 삭제(remove - `rm`)

### 1. `push` 명령어

```bash
$ git push origin master 
```

* 현재 폴더를 그대로 업로드 하는 것이 아니라, 지금까지의 이력/ 버전(commit)을 `push`하는 것이다. 
* Working directory, Staging area의 변경사항들은 원격저장소로 `push`되지 않는다.
* 따라서, `push`전에 `$ git status` , `$ git log` 를 통해서 확인하는 습관을 가지자. 

### 2. `pull` 명령어

```bash
$ git pull origin master
```

* 원격 저장소 변경사항(이력 )



## Git 원격 저장소 활용(github)

* Github에 repository를 생성한다.

  >  부제: 집에서 수업내용 복습하는 법 

  * 집에 git 저장소가 없는 경우 `clone`을 받는다. 

    ```bash
    $ git clone {__url__}
    ```

### 시나리오 

1. 멀티 캠퍼스 

   1. `commit`

      * 수업 종료 후 commit

      ```bash
      $ git commit -m '4/10 - 케라스 활용 방법'
      ```

   2. `push` 

      ```bash
      $ git push origin master
      ```

2. 집

   1. `pull`

      ```bash
      $ git pull origin master
      ```

   2. 복습 

   3. `commit`

      ```bash
      $ git commit -m '4/10 - keras복습 (블로그 참고)'
      ```

   4. `push`

      ```bash
      $ git push origin master
      ```

3. 다음날

   ```bash
   $ git pull origin master
   ```

   * 수업을 잘 듣는다. 

### 원격 저장소 활용시 주의사항 

* 원격 저장소와 로컬 저장소의 이력이 다른 경우 아래와 같이 나타난다. 
  * 예) pull을 하지 않고, 집에서 commit 하고 push 하는 경우 

```bash
$ git push origin master
To https://github.com/edutak/git.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/edutak/git.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. 
# 너는 아마도 원할 걸..? 
# 원격저장소(remote) 변화들(changes) 통합
# push를 다시 하기전에...
You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```



# CLI

> Command Line Interface에서는 명령어와 그 결과를 항상 주의하자.

1. `ls`- 디렉토리 목록 

   ```bash
   $ ls
   a.txt  b.txt   image/
   ```

2. `cd` - 디렉터리 변경 

   ```bash
   ~/Desktop/ $ cd image
   ~/Desktop/image/ $cd .. 
   ~/Desktop/ $ pwd
   /c/Users/student/Desktop/
   ```

   * `.` :  현재디렉토리 
   * `..` : 상위 디렉토리
   * `~` : 홈 디렉토리 
     * 위의 상황에서는 C드라이브의 Users 폴더 내부의 students 폴더

3. `pwd` - 현재 작업 디렉토리 

   ```bash
   ~/Desktop/ $ pwd 
   /c/Users/student/Desktop/
   ```

   

# Vi(Vim)

> CLI 환경에서 쓸 수 있는 텍ㅎ스트 에디터 중 하나 

commit 하는 과정에서 메시지 옵션을 쓰지 않으면, 나타난다. 

```bash
$ git commit
```

* 편집 모드(`i`) - 텍스트 편집 
* 명령 모드(`esc`)
  * `:wq`
  * `w` : write (저장)
  * `q` : quit (종료)



# Git 프로젝트 관리 

## 1. .gitignore

> git 으로 관리하지 않고 싶은 파일 목록은 `.gitignore`에 정의한다. 

```bash
*.xlsx		# 특정 확장자
secret.txt  # 특정 파일 
tests/		# 특정 폴더
```

* 주의 : 확장자 없음
* 만약, 어떤 파일을 일반적으로 등록하는 지 모른다면, [gitignore.io](https://gitignore.io) 을 참고하자. 
  * 예) `ipynb_checkpoints/`

## 2. Git- Ifs

> 용량이 큰 파일을 관리하기 위해서는 `git-Ifs` 설정이 필요하다. 

* https://git-Ifs.github.com/ 참고 할 것.
* 주의 : 해당 파일이 이미 커밋 되면 다른 작업을 거쳐야 함. 





# Branch 

> branch 기능은 서로 다른 독립된 작업 이력을 관리할 수 있도록 한다. 

* [git flow](https://nvie.com/posts/a-successful-git-branching-model/)



## 1. branch 관련 기초 명령어

1. branch 생성

   ```bash
   $ git branch {브랜치이름}
   ```

2. branch 이동 

   ```bash
   (master) $ git checkout {브랜치이름}
   (브랜치이름) $
   ```

3. branch 생성 및 이동 

   ```bash
   $ git checkout -b {브랜치이름}
   (브랜치 이름)$
   ```

   

4. branch 목록 

   ```bash
   (master) $ git branch
   *master 
   브랜치이름 
   ```

5. branch 병합 

   ```bash
   (master) $ git merge {브랜치이름}
   ```

6. branch 삭제 

   ```bash
   $ git branch -d {브랜치이름}
   ```

## 2. branch 병합 시나리오 

> branch 관련된 명령어는 간단하다. 
>
> 다양한 시나리오 속에서 어떤 상황인지 파악하고 자유롭게 활용할 수 있어야한다. 

### 상황1. fast- foward 

> fast- forward는 feature 브랜치 생성된 이후 master 브랜치에 변경사항이 없는 상황 

1. featuer / data - clean branch 생성 및 이동 

   ``` bash
   (master) $ git checkout -b feature/data-clean
   Switched to a new branch 'feature/data-clean'
   (feature/data-clean) $
   ```

2.  작업 완료 후 commit 

   ```bash
   $ touch clean.txt
   $ git add .
   $ git commit -m "Complete clean"
   ```

3. master 이동 

   ```bash
   (feature/data-clean) $ git checkout master
   Switched to branch 'master'
   (master) $
   ```

4. master에 병합

   ```bash
   (master) $ git merge feature/data-clean
   ```

5. 결과 -> fast-foward (단순히 HEAD를 이동)

   ```bash
   (master) $ git merge feature/data-clean
   git merge feature/data-clean 
   Updating 1036e68..98079f0
   Fast-forward
    clean.txt | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 clean.txt
   ```

6. branch 삭제

   ```bash
   $ git branch -d feature/data-clean 
   Deleted branch feature/data-clean (was 98079f0).
   ```



---

### 상황 2. merge commit

> 서로 다른 이력(commit)을 병합(merge)하는 과정에서 **다른 파일이 수정**되어 있는 상황
>
> git이 auto merging을 진행하고, **commit이 발생된다.**

1. feature/tensorflow branch 생성 및 이동

   ```bash
   (master) $ git checkout -b feature/tensorflow
   Switched to a new branch 'feature/tensorflow'
   (feature/tensorflow) $
   ```

   

2. 작업 완료 후 commit

   ```bash
   $ touch tf.txt
   $ git add .
   $ git commit -m 'Complete TF'
   ```

   

3. master 이동

   ```bash
   $ git checkout master
   ```

4. *master에 추가 commit 이 발생시키기!!*

   * **다른 파일을 수정 혹은 생성하세요!**

   

5. master에 병합

   ```bash
   (master) $ git merge feature/tensorflow
   ```

   

6. 결과 -> 자동으로 *merge commit 발생*

   * git log로 확인 해보세요!



7. 그래프 확인하기

   ```bash
   $ git log --oneline --graph
   *   dfc05ab (HEAD -> master) Merge branch 'feature/tensorflow'       
   |\
   | * 9b97ef6 (feature/tensorflow) Complete Tensorflow
   * | 610325b hotfix
   |/
   * 98079f0 Complete clean
   * 1036e68 Init
   ```

   

8. branch 삭제

   ```bash
   $ git checkout -d feature/tensorflow
   ```

   



---

### 상황 3. merge commit 충돌

> 서로 다른 이력(commit)을 병합(merge)하는 과정에서 **같은 파일의 동일한 부분이 수정**되어 있는 상황
>
> git이 auto merging을 하지 못하고, 충돌 메시지가 뜬다.
>
> 해당 파일의 위치에 표준형식에 따라 표시 해준다.
>
> 원하는 형태의 코드로 직접 수정을 하고 직접 commit을 발생 시켜야 한다.

1. feature/visualization branch 생성 및 이동

   ```bash
   $ git checkout -b feature/visualization
   ```

2. 작업 완료 후 commit

   ```bash
   $ touch visual.txt
   # README.md 수정
   # 아래 상태 메시지로 확인하세요.
   $ git status
   On branch feature/visualization
   Changes not staged for commit:                        alization)
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in walization)orking directory)
           modified:   README.md
   
   Untracked files:
     (use "git add <file>..." to include in what will be 
   committed)
           visual.txt
   
   no changes added to commit (use "git add" and/or "git 
   commit -a")
   $ git add .
   $ git commit -m 'Complete visual'
   ```

   


3. master 이동

   ```bash
   $ git checkout master
   ```

   


4. *master에 추가 commit 이 발생시키기!!*

   * **동일 파일을 수정 혹은 생성하세요!**

   

5. master에 병합

   ```bash
   $ git merge feature/visualization 
   Auto-merging README.md
   CONFLICT (content): Merge conflict in README.md
   Automatic merge failed; fix conflicts and then commit the result.
   ```


6. 결과 -> *merge conflict발생*

   > git status 명령어로 충돌 파일을 확인할 수 있음.

   ```bash
   (master|MERGING) $ git status
   On branch master
   # 충돌 난 곳이 있음.
   You have unmerged paths.
     # 고치고 나서 commit을 실행 시킬 것.
     (fix conflicts and run "git commit")        
     (use "git merge --abort" to abort the merge)
   
   Changes to be committed:
           new file:   visual.txt
   
   # README.md 이 동시에 수정됨.
   # 해결하고 add 할 것..
   Unmerged paths:
     (use "git add <file>..." to mark resolution)
           both modified:   README.md
   ```


7. 충돌 확인 및 해결

   ```
   <<<<<<< HEAD
   * 분석 내용 정리
   =======
   * 시각화 작업
   >>>>>>> feature/visualization
   ```


8. merge commit 진행

   > 2번 상황에서는 merge commit 이 발생하였지만, 그 중간 과정에서 충돌이 났기 때문에 직접 변경사항을 add하고 commit을 하여야 합니다.

   ```bash
   $ git add .
   $ git commit
   ```

   * vim 편집기 화면이 나타납니다.

   * 자동으로 작성된 커밋 메시지를 확인하고, `esc`를 누른 후 `:wq`를 입력하여 저장 및 종료를 합니다.
     * `w` : write
     * `q` : quit

   * 커밋이  확인 해봅시다.

9. 그래프 확인하기

   ```bash
   $ git log --oneline --graph
   *   891aafc (HEAD -> master) Merge branch 'feature/visualization'   
   |\
   | * 2d2e099 (feature/visualization) Complete Visual
   * | 31eaefc Update markdown
   |/
   *   dfc05ab Merge branch 'feature/tensorflow'
   |\
   | * 9b97ef6 Complete Tensorflow
   * | 610325b hotfix
   |/
   * 98079f0 Complete clean
   * 1036e68 Init
   ```


10. branch 삭제

    ```bash
    $ git branch -d feature/visualization
    ```


# Stash

> 작업 내역을 저장할 수 있는 공간이 있다.

## 기본 명령

1. stash 보관

   ```bash
   $ git stash
   ```

2. stash 반영

   ```bash
   $ git stash pop 
   # $ git stash apply - 반영
   # $ git stash drop  - 삭제
   ```

3. stash 목록

   ```bash
   $ git stash list
   ```



## 상황

> git은 commit 되지 않은 변경 사항에 대해서는 되돌리 수 없다.
>
> 따라서, 작업 내역이 있을 때(WD/Staging area 상태인 작업) 병합 과정이 진행되지 않는다. (merge, pull)

```bash
$ git pull origin master
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), done.
From https://github.com/edutak/1001-ai
 * branch            master     -> FETCH_HEAD
   38ea6fa..64e06b8  master     -> origin/master

# pull 내용을 받아오면, 지금 로컬 변경사항 중 다음 파일이 덮어씌여질 것이다.
error: Your local changes to the following files would be overwritten by merge:
        README.md
# 커밋을 하거나
# stash를 해라.
Please commit your changes or stash them before you merge.
Aborting
Updating 38ea6fa..64e06b8

```

* 해결 과정

```bash
student@M1303 MINGW64 ~/Desktop/백일장 (master)
$ git stash
Saved working directory and index state WIP on master: 38ea6fa Init

student@M1303 MINGW64 ~/Desktop/백일장 (master)
$ git stash list
stash@{0}: WIP on master: 38ea6fa Init

student@M1303 MINGW64 ~/Desktop/백일장 (master)
$ git pull origin master
From https://github.com/edutak/1001-ai
 * branch            master     -> FETCH_HEAD
Updating 38ea6fa..64e06b8
Fast-forward
 README.md | 22 +++++++++++-----------
 1 file changed, 11 insertions(+), 11 deletions(-)

student@M1303 MINGW64 ~/Desktop/백일장 (master)
$ git stash pop
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
The stash entry is kept in case you need it again.
```



# 명령어 취소 및 되돌리기

1. `add` 취소

   ```bash
   $ git restore --staged {파일명}
   ```

   * `add` 명령을 취소하는 작업

   * Staging area => Working directory

   * 구버전 명령어

     ```bash
     $ git reset HEAD {파일명}
     ```

2. 커밋 메시지 변경

   **주의!!! 커밋 해시값이 바뀌므로, `push` 한 이후에는 절대 하면 안됨.**

   ```bash
   $ git commit --amend
   ```

   * vim 편집기 창에서 직접 메시지를 수정하고, 저장(`:wq`)하면 된다.

3. 커밋 변경

   * 만약, 특정 파일을 추가하지 못한 상태로 커밋을 하였다면, 아래와 같이 해결해서 커밋에 포함시킬 수 있다.

     ```bash
     $ git add omit_file.txt
     $ git commit --amend
     ```

   * **당연하게도, 커밋 해시값이 변경되므로 이미 `push` 한 상태에서는 하지 말 것!**

4. working directory 변경사항 삭제

   **주의!! 아래의 명령어를 입력하면, 절대 되살릴 수 없다.**

   ```bash
   $ git restore {파일명}
   ```

   * 구버전 명령어

     ```bash
     $ git checkout -- {파일명}
     ```

5. `reset` vs `revert`

   * 두 명령어는 특정 시점의 상태로 이력(커밋) 되돌리는 작업을 한다.

   * `reset` : 이력을 삭제

     * `--hard` : 해당 커밋 이후 변경사항 모두 삭제 (주의)
     * `--soft` : 해당 커밋 이후 변경사항 및 working directory 내용까지 보관
     * `--mixed` : (default) 해당 커밋 이후 변경사항 staging area 보관

     ```bash
     $ git log --oneline
     a70327e (HEAD -> master, origin/master) Update a.txt
     41a723a Init
     ```

   * `revert` : 되돌렸다는 이력을 남긴다.

     ```bash
     $ git log --oneline
     1cd8c64 (HEAD -> master) Revert "Update a.txt"
     b696a08 Complete a, b
     c02b94a R
     a70327e (origin/master) Update a.txt
     41a723a Init
     ```

     

















