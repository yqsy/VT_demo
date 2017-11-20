
<!-- TOC -->

- [1. 代码出处](#1-代码出处)
- [2. 整理过程](#2-整理过程)

<!-- /TOC -->



<a id="markdown-1-代码出处" name="1-代码出处"></a>
# 1. 代码出处
https://bbs.pediy.com/thread-214916.html


<a id="markdown-2-整理过程" name="2-整理过程"></a>
# 2. 整理过程

```bash
# 查看后缀
find . -not -path "./.git/*" -type f | perl -ne 'print $1 if m/\.([^.\/]+)$/' | sort -u

# 查看编码
find . -name '*.asm' -type f -print0 | xargs -0 file

# 发现需要整理的后缀为
asm;c;h

# 空格转换tab
find . -name '*.asm' -type f -exec bash -c 'expand -t 4 "$0" > /tmp/e && mv /tmp/e "$0"' {} \;
find . -name '*.c' -type f -exec bash -c 'expand -t 4 "$0" > /tmp/e && mv /tmp/e "$0"' {} \;
find . -name '*.h' -type f -exec bash -c 'expand -t 4 "$0" > /tmp/e && mv /tmp/e "$0"' {} \;

# 转换成LF(去BOM)
find . -name '*.asm' -type f -print0 | xargs -0 dos2unix
find . -name '*.c' -type f -print0 | xargs -0 dos2unix
find . -name '*.h' -type f -print0 | xargs -0 dos2unix

# 转编码成GB18030
find . -name '*.asm' -type f -exec bash -c 'iconv -t GB18030 "$0" > /tmp/e && mv /tmp/e "$0"' {} \;
find . -name '*.c' -type f -exec bash -c 'iconv -t GB18030 "$0" > /tmp/e && mv /tmp/e "$0"' {} \;
find . -name '*.h' -type f -exec bash -c 'iconv -t GB18030 "$0" > /tmp/e && mv /tmp/e "$0"' {} \;

# 风格
find . -name "*.c" -type f -print0 | xargs -0 clang-format -i
find . -name "*.h" -type f -print0 | xargs -0 clang-format -i
```