# linux command cheat sheet

```
## rm -rf 여러개
$ rm -rf a.log b.log

## 내가 찾으려는거 after 10줄, before 10줄
$ cat a.log | grep 'Exception' -a 10 -b 10

## 디렉토리 이름변경
$ mv [oldName] [newName]

## 파일찾기
$ sudo find / -name nginx
$ sudo find / -name ‘*jdk*’ 

## etc.
$ ls -al
$ ls -alF

$ cat README.md > test.txt
```

# vi

```
## vi line number
:set number		#on
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