---
layout: post
title: "Title"
comments: true
categories: 
published: true

---

# Coursera Web Intelligence Course

```
$ iconv -f latin1 -t utf-8 original/* | sed -e 's/\\/\\\\/g' -e 's/"/\\"/g' -e 's/^/{"paper-id":"/' -e 's/:::/", "Authors": ["/' -e 's/:::/"], "Title":"/' -e 's/\r/"}/' -e 's/::/","/g' | iconv -f utf-8 -t latin1
```

```
$ mongoimport --db bigdata --collection hw3a < hw3.json
connected to: 127.0.0.1
Fri Apr 19 23:42:31.010                         34100   11366/second
Fri Apr 19 23:42:32.104 check 9 49137
Fri Apr 19 23:42:32.209 imported 49137 objects
```


