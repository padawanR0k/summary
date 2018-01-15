## 커널
	소프트웨어 - 커널 - 하드웨어 이어줌

## 쉘
	커널 - 쉘 - 사용자  를 이어줌
### bash
	- ~ : 내가 어딨는지 알려주는 부분	
	
   #### 명령어
   - cd : 원하는 폴더로 
   - ls : 현재위치의 파일,폴더리스트
   - ls -l : 리스트르 한줄하줄 보여줌
   - ls -a : 숨김 파일까지 보여줌
   - mkdir 폴더이름 : 폴더 생성 
   - touch 파일이름 : 파일 생성
   - touch .파일이름 : 숨김 파일 생성
   - cp 파일이름 원하는위치 : 파일을 원하는 위치로 복사
   - mv 파일이름 원하는위치 : 파일을 원하는 위치로 이동
   - mv 파일이름 원하는이름 : 파일명 변경	 
   - rm 파일이름 : 파일 삭제
   - rm -rf 폴더이름 : 폴더이름 	 
   - sudo : 슈퍼유저 권한얻음
   - exit : 쉘 종료

# VI

## 저장 및 종료


### 명령어
- 저장
---
	:w

- file.txt 파일로 저장 	
---
	:w file.txt

- file.txt파일에 덧붙여서 저장
--- 
	:w >> file.txt
- vi 종료
--- 
	:q
- vi 강제종료
---
	:q!
- 저장 후 종료
---
	:ZZ
- 강제 저장후 종료
---
	::wq!
- file.txt 파일을 불러옴
---
	:e file.txt
- 현재 파일을 불러옴
--- 
	:e
- 바로이전에 열었던 파일을 불러옴
---
	:e#
### 입력모드 전환
- a : 커서위치 다음칸 부터 입력
- A : 커서 행의 맨 마지막부터 입력
- i : 커서위치에 입력
- I : 커서 행의 맨앞에서부터 입력
- o : 커서 다음행에 입력
- O : 커서이전 행에 입력
- s : 커서 위치의 한글자를 지우고 입력
- cc : 커서 위치의 한행을 지우고 입력 


---
### [더 알아보기](http://gyuha.tistory.com/157)
---

# Git
- 빠르다
- 분산형 저장소 지원
- 비선형적으로 개발가능
	- 깃에 올릴 때는 동작하는 단위로 올려야한다.	
    
## Git inside 
- blob: 모든파일이 Blob이라는 단위로 구성
- commit: 파일에 대한 정보를 모은것
- tree : blob 을 모은것.

## 명령어
- add : 깃 추가
- commit : 커밋 상태를 추가함
- push : 커밋한 것을 보냄
- remote : 보낼 주소를 정함 

## TIP
- git init 을 할땐 위치를 고려하자. (.git이라는 파일이 생성됨)    
- .git 파일을 지우면 더 이상 깃이 아니게 된다.
- .gitignore : 내가 트랙을 원치않는 파일이나 폴더를 설정해 놓으면 깃이 트랙하지않는다.
- **add** 하고 **commit** 한다음 **push** 하자  

## branch
분기점을 생성하고 독립적으로 코드를 변경할 수 있도록 도와주는 모델
	
 	마스터브랜치에서는 작업을 하는곳이아니라 배포를 하는곳. 
    

- $ git branch 
- $ git branch -r
- $ git branch -a
- $ git branch develop : 만듦
- $ git branch checkout : 체크아웃 이동

## 강의영상
1. https://www.youtube.com/watch?v=Z4Lb3_Rlst0&feature=em-share_video_user
2. https://www.youtube.com/watch?v=iA82gjFMnjA&feature=em-share_video_user
3. https://www.youtube.com/watch?v=Qj_cMYJdovw&feature=em-share_video_user
4. https://www.youtube.com/watch?v=JyPFfQ-y6-U&feature=em-share_video_user

## fork 
- 다른 사람의 깃허브에서 레포지를 복사해온다.



