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

> CLI 환경에서 쓸 수 있는 텍스트 에디터 중 하나 

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

