---
layout: post
title: "Pythons (2) and unicode"
date: 2014-04-05 13:25:16 +0100
comments: true
categories:
---

[Repeat after me][1]:  Each unicode encoding (UTF-8, UTF-7, UTF-16, UTF-32, etc) maps different sequences of bytes to the unicode code points (therefore might as well map same sequences of bytes to different unicode code points)

Types:

- The `unicode` type stores an abstract sequence of code points. Each code point represents a grapheme.
- `str` is for strings of bytes. These are very similar in nature to how strings are handled in C.

[```uni.encode(encoding): Unicode string to a Python byte string
s.decode(encoding): convert a byte string to a Unicode string
unicode(s, encoding): convert a byte string to a Unicode string
```][2]

Important - [Python 3][3]. Important not only for guidelines but also for [clarity][4]. Moreover many "solutions" floating on the web are Python 3 ([`open()`][3a] has __both__ an encoding _and_ newlines param in P3. ). Speaking of 2:

    with open("file.txt", 'wb')as out:
        out.write('\n')
    with open("file2.txt", 'w')as out:
        out.write('\n')
    print(os.stat('file.txt').st_size)  # 1
    print(os.stat('file2.txt').st_size)  # 2


Refs:

- [Unicode In Python, Completely Demystified][5] - best slides I've seen, if you have the template comment !
- [Strip newlines][6]
- [os.linesep][7]
- `raise MyException(u'Cannot do this while at a café')`
- [the standard library remains ASCII-only with the exception of contributor names in comments.][3]
- [Ref][8]
- [Encode and decode as needed][9]

[1]: https://pythonhosted.org/kitchen/unicode-frustrations.html
[2]: https://stackoverflow.com/questions/10288016/usage-of-unicode-and-encode-functions-in-python/10288438#10288438
[3]: https://docs.python.org/dev/whatsnew/3.0.html#text-vs-data-instead-of-unicode-vs-8-bit
[3a]: https://docs.python.org/3.1/library/functions.html#open
[4]: https://stackoverflow.com/questions/10288016/usage-of-unicode-and-encode-functions-in-python/10288438#comment13236781_10288016
[5]: http://farmdev.com/talks/unicode/
[6]: http://stackoverflow.com/questions/339537/end-line-characters-from-lines-read-from-text-file-using-python/339842#339842
[7]: http://stackoverflow.com/questions/1223289/how-to-write-native-newline-character-to-a-file-descriptor-in-python
[8]: https://docs.python.org/2/howto/unicode.html
[9]: http://stackoverflow.com/a/12764646/281545
