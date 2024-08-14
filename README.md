# 시스템 프로그래밍 실습 1 - 1 과제

### 이름 : 이준휘

### 학번 : 2018202046

### 교수 : 최상호 교수님

### 강의 시간 : 화

### 실습 분반 : 목 7 , 8


## 1. Introduction

```
첫 번째 과제에서 mkdir()을 사용하였을 때 기본 설정된 권한으로 인해 rwxrwxrwx 권한
이 부여되지 문제가 발생한다. 이를 umask()를 통해 기본 설정된 권한을 변경하는 문제가
주어져있다. 두 번째 과제로는 웹사이트를 입력 받아 이를 주어진 hash 변환 함수를 이용
하여 변환한다. 저장되는 주소는 기본적으로 현재 사용중인 user의 home directory에
cache directory에 저장된다. 만약 cache directory가 없을 경우 새로 생성한다. 변환된 데
이터에서 처음 일부를 directory 명으로 사용하고, 일부는 해당 directory 안에 파일로 저
장된다.
```
## 2. Flow Chart


## 3. Pseudo Code

```
main(void){
```
```
char[100] input, home_dir, temp_dir;
```
```
char[60] hashed_url;
```
```
char[4] first_dir;
```
```
char *dir_div;
```
```
FILE *temp_file
```
```
home_dir = ~/cache;
```
```
set umask 000;
```
```
make home_dir directory(permission = drwxrwxrwx);
```
```
while(true){
```
```
input = User’s input;
```
```
if(input is ‘bye’) end_program;
```
```
hashed_url = hashed input using sha1;
```
```
first_dir = { hashed_url[0~3], ‘\0’ };
```
```
dir_div = hashed_url + 3 address
```
```
temp_dir = ~/cache/first_dir;
```
```
make temp_dir directory(permission = drwxrwxrwx);
```
```
temp_dir = ~/cache/first_dir/dir_div
```
```
temp_file = make temp_dir file and open file;
```
```
temp_file close;
```
```
}
```
```
}
```
## 4. 결과 화면


- 1

#### 해당 코드는 기본적으로 주어진 코드다. 해당 코드를 실행하였을 때의 결과는 아래

#### 와 같다.

```
결과를 보았을 때 기존에 mkdir()를 통해 권한을 777 로 설정하였지만 실제 권한은
775 로 설정된 것을 볼 수 있다. 이는 기본적으로 존재하는 권한 umask()가 002 로
설정되어 있기 때문에 777 - 002 = 775(rwxrwxr-x) 권한이 설정되는 것이다.
```

```
이를 해결하기 위해 mkdir를 하기에 앞서 umask() 함수를 통해 기본 권한을 000 으
로 설정을 한 모습이다. 이를 실행하면 다음과 같다.
```
```
해당 코드를 살펴보면 만들어진 directory는 정상적으로 777(rwxrwxrwx)권한이 설
정된 것을 확인할 수 있다.
```
- 1 - 1


해당 파일은 proxy_cache.c이다. 우선 sha1_hash 함수를 살펴보면 input_url을 받아
서 hashed_url에 저장하고 출력하는 양식을 가지고 있다. 변환의 경우
openssl/sha.h 헤더에서 제공하는 SHA1()함수를 사용하여 번환하고, unsigned char
형 160 bit로 변환된 값을 char형으로 변환하기 위한 for문을 도는 모습을 볼 수 있
다.

#### 해당 함수 또한 위의 함수와 마찬가지로 기본적으로 주어졌다. 해당 함수는

getpwuid(getuid())를 통해 현재 사용자의 정보를 가져와 passwd 구조체에 넣으며
여기에서 home directory를 찾아 출력하는 모습을 볼 수 있다.


위의 사진은 메인 함수를 나타낸 것이다. getHomeDir()함수를 통해 현재 위치를 불
러와 home_dir에 저장하고 strcat()함수를 통해 뒤쪽에 /cache를 붙인다. 그 후
umask를 통해 기본 권한을 000 으로 설정, mkdir() 통해 cache폴더가 없을 경우 만
든다. while문에서는 URL을 받고 이를 sha1_hash()함수를 통해 변환 후 일부의 이름
은 directory로, 일부는 file명으로 저장하는 역할을 수행한다. 파일의 생성은
fopen()함수를 활용하여 제작되었다.

해당 그림은 Makefile의 내용을 표시한 것이다. 기존의 Makefile과 다른 점이 있다
면 SHA 변수를 통해 - lcrypto 옵션이 사용된다는 점인데, 이는 SHA1()함수를 쓰기
위해서 필수적으로 요구된다.


```
위의 사진은 실행 결과를 나타낸 것이다. make을 통해 정상적으로 파일이 생성되
고, 기존에 cache directory가 없는 상태에서 이를 실행하면 정상적으로 생성되는
것을 볼 수 있다. 또한 url을 입력하면 해당하는 url의 앞 3 글자는 directory명으로,
뒤의 글자는 파일 명으로 생성되는 모습을 볼 수 있다. 한 번 더 실행하였을 때에
는 cache폴더가 기존에 있는 상황이며 이 상황에서도 마찬가지로 정상적으로 url이
추가가 되는 모습을 볼 수 있다.
```
## 5. 고찰

```
해당 과제를 진행하면서 우선 시스템 프로그래밍 과목에서 배웠던 mkdir()와 umask()를
사용하였다. 이로 인하여 해당 함수의 사용을 익히고 이를 활용할 수 있었다. 또한 코드
를 작성하면서 vi 편집기를 최대한 활용하여 프로그래밍을 함으로써 3 주차에서 익혔던 내
용을 이용할 수 있는 과제였다. tree 명령어를 설치하고 사용하면서 파일의 구조를 쉽게
확인하는 방법을 알 수 있었다. 마지막으로 strcat과 strcpy 등의 문자열 함수를 사용하여
문자열을 쉽게 조작하는 법을 터득할 수 있는 과제였다.
```
## 6. Reference

### 강의 자료만을 참고


