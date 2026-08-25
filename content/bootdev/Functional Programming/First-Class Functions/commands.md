+++
date = '2026-08-25T13:40:12+02:00'
draft = false
title = 'Commands: split, strip, join...'
showAuthor = false
weight =53
layout = "simple"
summary = "🚀 commands: split, strip, join..."
+++

> str.split(sep=None, maxsplit=-1)

```python
'1,2,3'.split(',')
# ['1', '2', '3']

'1,2,3'.split(',', maxsplit=1)
# ['1', '2,3']

'1,2,,3,'.split(',')
# ['1', '2', '', '3', '']

'1<>2<>3<4'.split('<>')
# ['1', '2', '3<4']

'1 2 3'.split()
# ['1', '2', '3']

'1 2 3'.split(maxsplit=1)
# ['1', '2 3']

'   1   2   3   '.split()
# ['1', '2', '3']

"".split(None, 0)
# []

"   ".split(None, 0)
# []

"   foo   ".split(maxsplit=0)
['foo   ']

```

> str.strip(chars=None, /)

```python
'   spacious   '.strip()
# 'spacious'

'www.example.com'.strip('cmowz.')
# 'example'

comment_string = '#....... Section 3.2.1 Issue #32 .......'
comment_string.strip('.#! ')
# 'Section 3.2.1 Issue #32'

```

> str.join(iterable, /)

```python
', '.join(['spam', 'spam', 'spam'])
# 'spam, spam, spam'

'-'.join('Python')
# 'P-y-t-h-o-n'

```
