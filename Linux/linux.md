
# 리눅스 기초  

## 파일 시스템 이용을 위한 명령어  

/  
root. 시스템의 시작점  

..  
상위 디렉토리  

~  
home. 로그인한 유저의 home 경로.  환경변수로 바꿀 수 있다.  

.  
현재 디렉토리  

/  
디렉토리 구분자  

\-  
이전위치  

pwd  
Print Work Directory.  
현재 터미널이 위치한 디렉토리 경로 표시.  

ls  
List Segments.  
디렉토리의 파일 정보 표시.  
ls -al : 숨김파일과 파일의 모든 정보 표시. (ll alias로 사용가능)  
ls -il : 파일 또는 디렉토리의 inode number(고유 식별 부호)를 표시.  

cd  
Change Directory. 지정한 디렉토리로 이동.  
현재 위치부터 상대 경로로 이동(기본), root(/)부터 모든 경로 입력시 절대 경로로 이동.  

mkdir  
Make Directory. 새폴더 생성.  
root(/)부터 경로 입력시 절대 경로에 디렉토리 생성.  

rm  
Remove. 지정 파일/디렉토리 삭제.  
-f 옵션으로 강제 삭제.  
-r(reculsive) 옵션으로 디렉토리 제거 가능.(rm -r, rmdir)  

df  
Disk Filesystem.  
디스크 공간에 대한 정보 확인.(used, free, mount 정보)  
-h : human readable 옵션과 함꼐 사용.  

du  
Dist Usage.  
디스크 사용량, 사용률 확인.  
-h, -s(summary. 해당 경로 하위 총합 표시), -d(max-depth 지정. 해당 depth만큼 펼쳐서 표시)  

chmod  
Change mode. 파일의 접근 권한 변경.  
권한 종류 = r(read), w(write), x(execute)  
권한 범위 = 소유자(u), 그룹(g), 그 외 사용자(o), 모든 사용자(a)  
변환 방법 = 추가(+), 제거(-), 지정(=)  
user, group, others 각각 3자리의 2진수로 표현된 파일 모드 지정 가능.  
<img width="477" alt="Screenshot 2023-05-27 at 12 55 25 PM" src="https://github.com/badro97/Studying_Skills/assets/49307262/a2ee93e4-d60c-45c0-ba24-c63982f9efe2">  

---  

## 파일 관리를 위한 명령어  

touch  
빈 파일 생성.  

cat  
파일 내용 출력.  

head, tail  
파일 또는 파이프된 데이터의 시작/마지막 부터 지정한 행까지 내용 출력.  

comm  
Compare.  
두 파일을 라인별로 비교.  

cmp  
두 파일을 바이트 단위로 비교한 결과를 stdout에 프린트.  

diff  
Difference.  
두 파일을 라인별로 비교 후 차이점만 표시.(a, c, d)  

ln  
심볼릭 링크 생성.  
심볼릭 링크 : 절대 경로 또는 상대 경로의 형태로 된 다른 파일이나 디렉토리에 대한 참조를 포함한 특별한 종류의 파일.  
ln -s $ORIGIN_PATH $TARGET_PATH  

alias  
다른 문자열에 대체하는 단어 지정.  
환경 변수와는 다름.  
alias hello="echo hello $@"  

cp  
Copy. 파일이나 디렉토리(-r) 복사.  

mv  
Move. 파일이나 디렉토리를 이동.  

---  

## 검색에 사용하는 명령어  

find  
현재 경로에서 $filename 의 이름을 가진 파일이나 디렉토리를 찾음.  
find . -name $filename  

which  
\$PATH 에 등록된 경로 중 주어진 이름의 실행 파일 위치를 찾음.  

grep  
주어진 택스트/정규 표현식 패턴에 일치하는 텍스트 검색.  

sed  
텍스트를 필터링하거나 변환하는 스트림 에디터.  
sed \`s/old_word/new_word/g\` target_file  
target_file 에서 old_word를 new_word로 모두 교체한 결과 출력.  

---  

## 시스템 활용을 위한 명령어  

uname  
이름, 버젼, 기타 시스템 정보 확인. uname -a  

ps  
현재 실행중인 프로세스 확인.  
ps -ef: 프로세스 상태.  
ps aux: 프로세스 상태 정보 + cpu, memory 사용률.  
ps aux | grep java -> java 라는 단어를 명령어나 파라미터로 넣은 프로세스 확인.  

kill  
process 에 signal 전달.  
kill -9(SIGKILL) $pid : 프로세스 강제 종료.  
kill -15(SIGTERM) $pid 또는 kill $pid : 종료 signal 전달.  
kill -2(SIGINT) $pid : interrupt를 의미하는 signal.  
process를 유지하고 있는 session에서 ctrl+c 를 누르면 이 signal이 전달됨.  

shutdown  
시스템 종료.  

halt  
시스템 강제 종료.  

reboot  
시스템 재시작.  

---  

## 네트워크 활용을 위한 명령어  

ss  
socket status.  
네트워크, 소켓의 사용, 주로 열려있는 포트 확인할 때 사용.  
내용이 긴 경우 pipe() 해서 less 또는 grep 등과 함께 사용.  
ss : 연결이 맺어진 (STATUS=ESTAB) 소켓 확인.  
ss -a : 열려있는 모든 소켓.  
ss -l : listening 중인 소켓만 표시.  
ss -t : TCP socket 확인, 필요에 따라 -l, -a 등과 함께 사용.  
ss -u : UDP socket 확인, 연결을 맺지 않는 UDP 특성상 항상 -a -l 등과 함께 사용.  
ss -s : 현재 소켓 상태의 통계.  
ss -p : process name 과 pid를 함꼐 표시.  
ss -e : extended output.  
ss -m : memory 사용량을 함께 표시. memory 표시는 skmem형식 (man ss | grep skmem) 으로 확인.  

---  

## I/O 관련 명령어  

echo  
터미널 콘솔에 텍스트 출력.  
echo $(command) 로 다른 명령어의 결과 확인 가능.  
echo 와 redirection 연결해서 사용.  

clear  
터미널 기존 내용 지우기.  

history  
지나간 터미널 명령어 기록 표시.  
로그아웃/세션 종료 시 확인 불가.  
history 에서 확인한 명령어의 숫자를 !number 형태로 쓰면 다시 불러올 수 있다.  

redirection  
출력 결과를 파일로 저장.  
\> : 기존의 파일 내용을 지우고 저장  
\>> : 기존 파일 내용 뒤에 저장  
< : 파일의 데이터를 명령어에 입력  
<< : 지정한 단어까지의 데이터를 명령어에 입력  

stdout, stderror  
Process의 표준 입출력 제어.  
1> : stdout을 지정된 파일에 저장  
2> : stderror를 지정된 파일에 저장  
2>&1 또는 &> : stderror를 stdout에 포함시켜 저장  
\> /dev/null 출력 결과를 제거  

---  

# 환경 변수의 이해  

환경변수  
운영체제 수준에서 선언하는 변수이다.  
운영체제 해당 환경에서 실행되는 프로세스가 모두 참조할 수 있다.  
대표적으로 다음과 같은 경우에 사용.  

- 변수에 자주 사용하는 경로 저장  
    - HOME  
- 기존에 있는 변수를 이용한 새로운 변수 저장  
    - PATH=$PATH:newpath  
- 프로세스가 구동중에 참조할 값을 미리 환경변수에 할당하고 프로세스 실행  
- 여러개의 프로세스가 참조해야하는 값을 환경변수에 할당
    - \$LD_LIBRARY_PATH  

export 환경변수명=값  
값을 환경변수로 임시 선언.  
시스템 재부팅 또는 로그아웃 시 환경 변수 값 사라짐.  

환경 변수를 특정 유저에게만 영구적으로 적용하고 싶은 경우  
~/.bash_profile 파일을 수정한다. ~/.bash_profile 은 user가 처음 login할 때 수행된다.  
단, bash shell로 접속했을 때만 동작한다. sh, zsh로 접속하면 동작X.  

환경 변수를 모든 유저에게 영구적으로 적용하고 싶은 경우  
/etc/profile 파일 수정.  
- sudo nano /etc/profile  
- export HELLO="hello_world"  
- echo \$HELLO  

\$PATH 란?  
운영체제가 명령어의 실행파일을 찾는 경로  
- 절대/상대 경로 없이, 단독으로 명령어를 수행할 수 있다는 것은 해당 명령어의 실행파일이 운영ㅊ제의 \$PATH에 등록된 디렉토리들 중에 포함되어 있다는 의미이다.  
- which가 명령어 파일 위치를 찾는 원리.  
- 내가 만든 실행파일의 위치도 \$PATH 에 경로를 등록하면, 경로를 매번 입력하거나 찾지 않아도 바로 쓸 수 있다는 의미.  

---  

# Shell Script  

Shell script  
unix shell의 command line interpreter가 실행할 수 있는 명령어(리스트)를 의미한다.  

파일 맨 위에 다음과 같은 표시가 있다면 shell script 파일이라 할 수 있다.  

#!/bin/sh  : /bin/sh (bourne shell)를 사용하는 shell script  

#!/bin/bash : /bin/bash를 사용하는 bash shell script  

#!/bin/zsh : /bin/zsh를 사용하는 zshell shell script  

#! 는 shebang 이라고 읽는다.  

Shell script 실행법  
- ./$FILENAME  
    - 로그인한 user의 +x 권한 유무 확인.  

## 변수  

영문, 숫자, _ 만 사용가능.  
Unix shell 의 변수명은 대문자로 작성하는 것이 convention이다.  
변수명=변수값 으로 변수 선언 가능.  
\$변수명 : 변수 사용  
readonly 변수명 : 읽기전용 변수로 선언(변수 값 변경 불가)  
unset 변수명 : 변수 할당 해제  
- Script 내에서 선언한 변수 뿐 아니라, 환경 변수, 쉘 변수(접속 shell에서 export로 선언한 변수)도 \$변수로 읽어올 수 있다.  

## 특수목적 변수  

\$$  
현재 shell의 프로세스 아이디  

\$0  
현재 script의 파일 이름  

\$n  
script 실행시 넘겨준 n번째 인자  

\$#  
script에 넣어준 인자의 개수  

\$*  
모든 인자를 ""로 감싸서 반환  

\$@  
각 인자를 ""로 감싸서 반환  

\$?  
마지막으로 실행된 명령어의 종료 상태  

\$!  
마지막 백그라운드 명령어의 프로세스 아이디  
