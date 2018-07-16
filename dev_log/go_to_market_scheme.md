# App Scheme

TIL

```
웹페이지 처리시 url 프로토콜이 http, https가 아닐 경우, OS에서 처리한다.
```
## ios

```
http://itunes.com/apps/{AppName}
itms://itunes.com/apps/{AppName}
itms-apps://itunes.com/apps/{AppName}
```
- http 형식을 사용하면 브라우저를 거쳐서 App Store로 이동한다.
- itms-apps 형식을 사용하면 브라우저를 거치지 않고 바로 앱스토어로 이동한다.

참고: [앱스토어 링크 알아내기](http://bongman.tistory.com/25)

## android

```
https://play.google.com/store/apps/details?id={AppPackageName}
market://details?id={AppPackageName}
```
안드로이드도 마찬가지다.