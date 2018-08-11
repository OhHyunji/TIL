# regex example

## url에서 params 만 골라내기

```
(?<=[?&;])foo=.*?($|[&;])
\bfoo=([^&]+)
(?<=[?&;]foo=).*?($|[&])
(\b[?&;]foo=).*?($|[&])
(?<=[?&;]foo=).*?($|[&;]) 
```
참고: [RegExr](https://regexr.com/2vuqj)

> javascript에서는 `new URL("string")` 해서 쓰는게 훨씬 더 편한것 같다. 필요해서 찾아봤었는데 막상 사용하지는 않음.
