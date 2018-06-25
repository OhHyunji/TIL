# sbt cheat sheet

## sbt 프로젝트 실행

```
$ sbt run
$ sbt test 	//테스트코드 실행
```

## sbt console

```
$ sbt
$ sbt update 	//프로젝트가 사용하는 라이브러리 다운로드
$ sbt compile	//소스코드 컴파일
```

## thrift 빌드하기

```
$ sbt
sbt> project isool-idl
[info] Set current project to isool-idl 

sbt>;clean;compile
```
target 아래에 파일들이 새로 생긴다.

## thrift 배포

```
$ sbt package	//배포가능한 jar파일 만든다.
$ sbt publish	//jar를 원격 저장소에 넣는다.(maven repo)
```
## jar 만들어서 실행하기

### 1. dependencies 다 불러와서 fat jar 만든다.

```
$ sbt wine-http/assembly
```

로그 맨 마지막에 보면 이렇게 묶어서 만든 `.jar` 파일 경로가 나온다. target 아래에 만들어졌다.

```
[info] Assembly up to date: /Users/azalea/dev/Wine/wine-http/target/scala-2.12/wine-http.jar
```

### 2. 이 `.jar`파일을 실행시킨다.

```
$ java -jar wine-http.jar
```

help 보면 `.jar`파일 띄울 때 줄 수 있는 옵션들이 있다.

```
$ java -jar wine-http.jar -help
```
옵션들 중 worker 갯수 관련 옵션들이 있는데, 아래와 같이 줄 수 있다.

```
$ java -jar wine-http.jar -com.twitter.finagle.netty4.numWorkers=32
```
twitterServer admin(:9990/admin)에서 워커 32개 뜬 것 확인할  수 있다.

