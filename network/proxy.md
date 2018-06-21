# Proxy

## curl 
curl로 요청 날려보기

```
$ curl -l {요청날릴 url}
```

curl로 프록시 태워서 요청 날려보기

```
$ curl -x {proxyHost:proxyPort} -l {요청날릴 url}
```

## JVM Options: proxy 

```
export JAVA_OPTS="-server -Xms1024M -Xmx1024M -Dsun.net.inetaddr.ttl=0"
```
요기에 이어서 proxy option들 줄 수 있다.

- -Dhttp.proxyHost, -Dhttp.proxyPort  
- -Dhttps.proxyHost, -Dhttps.proxyPort
- -Dhttp.nonProxyHosts, -Dhttps.nonProxyHosts
	- 여기에 지정한 호스트들은 proxy를 안탄다.
