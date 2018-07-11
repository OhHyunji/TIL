# window.location

TIL

```
히스토리에 안쌓고 이동할 때: replace()
히스토리에 쌓고 이동할 때: assign()
```

## The difference between replace() and assign()

replace()

- The replace() method replaces the current document with a new one.
- it removes the URL of the current document from the document history.
- it is **not possible** to use the "back" button to navigate back to the original document.

assign()

- it is **possible** to use the "back" button to navigate back to the original document.
- it is the same as:

```
window.location = "new Url"
window.location.assign("newUrl")
```

참고: [W3School - location.replace()](https://www.w3schools.com/jsref/met_loc_replace.asp)

