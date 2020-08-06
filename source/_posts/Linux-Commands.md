---
title: Linux Commands
date: 2020-07-23 18:33:49
author: bh7cw
top: true
cover: true
coverImg: /images/5.jpg
toc: true
summary: This post concludes useful Linux commands.
categories: Linux
tags:
  - Linux
---
## grep
search for pattern in filename
`grep pattern filename` 
-i: captial insensitive
-r: search all the file in the directory
`grep -r admin /etc/`
## wc
```
$ wc demo.txt
7459   15915  398400 demo.txt
lines  words  bytes
行数    单词数  字符数    
```

## sed
`sed 's/ /-/g' example.txt`
replace all the `space` with `-`
Hello This is a Test 1 2 3 4 -> Hello-This-is-a-Test-1-2-3-4

`sed 's/[0-9]/d/g' example.txt`
replace all the numbers with `d`
Hello This is a Test 1 2 3 4 -> Hello This is a Test d d d d

## sort
`sort example.txt`
example.txt
```
f
b
c
g
a
e
d
```

sort example.txt
```
a
b
c
d
e
f
g
```
`sort example.txt | sort -R` randomly
`sort example.txt | uniq` show unique lines
`sort example.txt | uniq -c` show unique lines and times

## cut
`cut -d " " -f2,7,9 example.txt` show the 2,7,9 words connected with " "
red riding hood went to the park to play -> riding park play

## echo
`echo -ne "Hello\nWorld\n"`
"\n" output
Hello
World

## fmt
`cat example.txt | fmt -w 20`: print each line with 20 words

## tr
cat example.txt | tr 'a-z' 'A-Z': change a-z -> A-Z
`cat example.txt | tr ' ' '\n'`: change ' ' -> '\n'

## nl
`nl -s". " example.txt `show the number of line
```
 1. Lorem ipsum
 2. dolor sit amet,
 3. consetetur
 4. sadipscing elitr,
 5. sed diam nonumy
 6. eirmod tempor
 7. invidunt ut labore
 8. et dolore magna
 9. aliquyam erat, sed
10. diam voluptua. At
11. vero eos et
12. accusam et justo
13. duo dolores et ea
14. rebum. Stet clita
15. kasd gubergren,
16. no sea takimata
17. sanctus est Lorem
18. ipsum dolor sit
19. amet.
```

## passwd
change password

## date
show current date

## cal
show the calendar for this month

## finger
`finger username` display the info of the user

## uptime
show the running time

## uname
`uname -a` show kernel info

## df
show usage of disk

## du
`du filename` show the size of directory and file
-s: only show the total size

## ps
`ps -u yourusername` show the processes

## kill
`kill PID`

## killall
`killall processname`

## top
show current process

## bg

## fg

## dig
`dig domain` dig the info of the domain

## wget
`wget file` download the file

## bash
```
${varname:-word}    # 如果varname存在且不为null，则返回其值; 否则返回word
${varname:=word}    # 如果varname存在且不为null，则返回其值;否则设置它，然后返回其值
${varname:+word}    # 如果varname存在并且不为null，返回word; 否则返回null 
${varname:offset:length}    # 执行子字符串扩展。它返回$ varname的子字符串，从offset开始，最多为length的字符
2.2 String Substitution
```

## string
```
${variable#pattern}         # if the pattern matches the beginning of the variable's value, delete the shortest part that matches and return the rest
${variable##pattern}        # if the pattern matches the beginning of the variable's value, delete the longest part that matches and return the rest
${variable%pattern}         # if the pattern matches the end of the variable's value, delete the shortest part that matches and return the rest
${variable%%pattern}        # if the pattern matches the end of the variable's value, delete the longest part that matches and return the rest
${variable/pattern/string}  # the longest match to pattern in variable is replaced by string. Only the first match is replaced
${variable//pattern/string} # the longest match to pattern in variable is replaced by string. All matches are replaced
${#varname}     # returns the length of the value of the variable as a character string
```

## expression
```
statement1 && statement2  # 两边的条件都为true
statement1 || statement2  # 其中一边为true

str1=str2       # str1 匹配 str2
str1!=str2      # str1 不匹配 str2
str1<str2       # str1 是否小于 str2
str1>str2       # str1 是否大于 str2
-n str1         # str1 不为空(长度大于 0)
-z str1         # str1 为空(长度为 0)

-a file         # 文件存在 
-d file         # 文件存在，是一个目录 
-e file         # 文件存在; 相同的-a 
-f file         # 文件存在，是一个常规文件（即不是目录或其他特殊类型的文件） 
-r file         # 你有读权限 
-r file         # 文件存在，不为空 
-w file         # 你有写权限 
-x file         # 你有文件的执行权限

file1 -nt file2     # file1 is newer than file2
file1 -ot file2     # file1 is older than file2

-lt     # 小于 
-le     # 小于或等于 
-eq     # 等于
-ge     # 大于或等于 
-gt     # 大于
-ne     # 不等于
```

Ref:
https://www.cnblogs.com/savorboard/p/bash-guide.html
