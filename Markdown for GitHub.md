# Cheatsheet: Markdown for GitHub

## Character Style

```
**bold text**
__bold text__

*italic*
_italic_

_Combined **text**_

~~strikethrough~~
```

## Link

```
[Descriptions](https://www.google.com)
```

## Images

```
![Alt text](/dir/photo.png)
```

## Headers

```
# <h1> Tag
## <h2> Tag
###### <h6> Tag
```

## Lists

### Unordered List

* Item 1
* Item 2
  * Subitem 2a
  * Subitem 2b

### Ordered List

1. Item 1
2. Item 2
  3. Subitem 2a
4. Item 3

## Quotes

> Start quote after a right-angled bracket.

## Inline Code

You can add any code `<div>` here.

## Syntax Highlighting

```javascript
function someFunction(arg) {
  if(arg) {
    $.facebox({div:'#foo'})
  }
}
```

```python
def foo():
  if not bar:
    return True
```



## Task Lists

- [x] @mentions, #refs, [links](https://www.123.com), complete item
- [ ] imcomplete item



## Tables

First Header | Second Header

----- | -----

Content 1| Content 2

Content 3 | Content 4



## GitHub Specific Functionalities

1. SHA-1 hashes will be automatically converted into a link.
2. Numbers refers to Issue/Pull Requests will be automatically converted in to links.
3. Mentioning: typing `@` symbol followed by a username, will notify that person.
4. Automatic linking: any URL will automatically be converted into a link.
5. Emoji support.

