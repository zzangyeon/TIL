< 기본 설정 > 
버전 관리할 폴더에서 git bash 실행

📌 자신의 깃허브 이름과 이메일 작성

git config --global user.name "[이름]"
git config --global user.email "[이메일]"
잘 적용됐는지 확인

git config --list
📌 원격 저장소와 로컬 저장소를 연결

origin - 원격저장소 이름설정(관례상 origin으로 통일)

git remote add [origin] [원격 저장소 URL] 
git remote -v : 원격 저장소와 연결이 잘 됐는지 확인

 

📌 원격 저장소의 데이터 로컬로 가져오기(처음)

git clone [원격 저장소 주소] ([내가 사용할 디렉터리 이름])
이름을 지정하면 그 디렉터리 이름으로 복사해옴 ( 안 적으면 현재 디렉토리)

< 기본 명령어 >
📌 현재 폴더 깃 초기화

폴더 이름을 적으면 디렉터리를 새로 만드는 동시에 깃 초기화

git init [폴더이름 or null]
📌 깃 상태 확인

git status
<출력되는 메시지>

on branch master : 현재 master 브랜치라는 뜻
no commits yet : 아직 커밋한 파일이 없음
nothing to commit : 현재 커밋할 파일이 없음
untracked files : 한 번도 (버전관리)하지 않은 파일이 존재함
Changes to be committed : 커밋 가능
Changes not staged for commit : 변경된 파일이 아직 스테이지에 올라가지 않았음
 

📌 파일을 스테이징 공간에 추가

. 은 모든 파일 추가(추가된 파일 수정된파일)

git add .                 
   or
git add [파일이름]
 

📌 커밋하기 

git commit -m "<메시지>"
git commit -am "[커밋메세지]" : 스테이징과 커밋 한꺼번에 처리(메시지도 같이)

git commit --amend : 방금 커밋한 메세지 수정하기

 

📌 커밋 이력 확인

git log (--stat) 
버전이 제대로 만들었는지 확인하기(관련 파일까지 확인하기)

git log --oneline : 커밋 내용 간략하게 한 줄에 한 커밋씩 살펴보기

git log --oneline --branches : 커밋 내용 간략하게 한 줄에 한 커밋씩 살펴보고 각 브랜치의 커밋을 함께 볼 수 있음

git log --oneline --branches --graph : 커밋 내용 간략하게 한 줄에 한 커밋씩 살펴보고 각 브랜치의 커밋을 함께 보

고 브랜치와 커밋의 관계까지 확인

git log <브랜치 1>.. <브랜치 2> : 브랜치 1을 기준으로 브랜치 2와 비교 후 브랜치 2에 없는 브랜치 1의 커밋을 출력

 

📌 해당 브랜치로 이동

git checkout [브랜치이름]
git checkout -b <브랜치이름> : 브랜치를 생성함과 동시에 생성된 브랜치로 이동

 

📌 변경사항 확인하기

git diff 
 

📌 스테이징 한 파일 되돌리기

git reset HEAD [파일이름] 
Unstaged : 스테이징 된 파일이 내려갔음
git reset HEAD^ : 최신 커밋 되돌리기
git reset --hard [커밋 해시] : 특정 커밋으로 되돌리기
 

📌 존재하는 브랜치 확인

git branch [브랜치이름]
git branch -d [삭제할 브랜치 이름] : 마스터에서만 가능하며 다른 로컬 브랜치를 삭제함, 영구 삭제는 아니고 다시 같은 이름으로 만들면 그대로 있음

 

📌 현재 헤더가 가리키는 브랜치와 다른 브랜치와 병합함, 옵션으로 편집창 안 뜨게 함

git merge [브랜치이름] (--no-edit) 
 

📌 커밋되기 전의 수정 파일을 숨겨놓기, 숨긴 파일들 리스트 보기

git stash (-list)  
 

📌 로컬에서 커밋한 내역 원격 저장소로 올리기

git push (-u) [origin] [master] 
-u는 로컬 저장소의 브랜치를 원격 저장소의 master 브랜치에 연결하기 위한 것으로 처음 한 번만 함
git push -d origin <브랜치이름> : 원격 저장소에 있는 브랜치를 삭제함
 

📌 원격 저장소의 변경된 사항을 로컬 저장소로 가져와 병합함

git pull
📌 원격 저장소의 변경된 사항을 로컬 저장소로 불러오기만 함( 병합X)

git fetch


블로그 원글 - https://zzangyeon.tistory.com/36?category=512841
