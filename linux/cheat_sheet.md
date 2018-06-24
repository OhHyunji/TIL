# Linux Command Cheat Sheet

## 파일 지우기

```
$ rm -rf a.log
$ rm -rf a.log b.log		# 여러개 한번에
```

## 파일 복사

```
$ cp [source] [destination]
$ scp [source] [destination]
```

## 파일 찾기

```
$ sudo find / -name nginx
$ sudo find / -name ‘*jdk*’ 
```
## 파일 리스팅 

```
$ ls -al
$ ls -alF

drwxr-xr-x  14 azalea  staff   448  6 21 20:43 TIL
```

- 파일타입+permission정보/링크수/소유자/소유그룹/용량/생성날짜/파일명

## 파일 권한

```
drwxr-xr-x
```

- 1: 파일/디렉토리
- 3개씩 User, Group, Others
	- permission: r, w, x(execute)
	- 파일타입: d(directory), l(링크파일), -(일반파일)


## grep after 10줄, before 10줄

```
$ cat a.log | grep 'Exception' -a 10 -b 10
```

## 디렉토리 이름변경

```
$ mv [oldName] [newName]
```

## command history

```
ctrl + r
```

## 압축/해제

```
$ tar -cvf [파일명.tar] [폴더명]
$ tar -xvf [파일명.tar]
```

## api 찔러보기

```
curl -X POST [url]
curl -X GET [url]
wget [url]
```

## bash_profile

```
$ vi ~/.bash_profile
$ source .bash_profile
```

## etc.

```
$ cat README.md > test.txt
```

## vi

```
## 코드 라인숫자 on/off
:set number			#on
:set nu				#on
:set nonumber		#off

## 찾아바꾸기
:s/aa/bb		#current line
:%s/aa/bb		#all file

/[찾을문자열]
n		#다음
N		#이전

ctrl+f		#다음페이지
ctrl+b		#이전페이지
```
