## 基础正则表达式字符

RE 字符  |  含义                    |  以 `grep` 查找命令为例
---------|--------------------------|-------------------------------------
^word    | 以 `word` 开始           | `grep -n '^#' file.txt'`  -- 从 file.txt 中查找以 # 开头的行，并列出行号
word$    | 以 `word` 结束           | `grep -n '!$' file.txt'`  -- 从 file.txt 中查找以 ! 结尾的行，并列出行号
.        | 代表一个或多个任意的字符 | `grep -n 'e.e' file.txt'`  -- 从 file.txt 中查找 e 和 e 之间有一定字符的行，并列出行号
c*       | 重复零个或多个前一个字符 | `grep -n 'ess*' file.txt'`  -- 从 file.txt 中查找含有 es、ess、esss 等字符串的行，并列出行号
[list]   | 字符只能出现在 list 集合中 | `grep -n 'g[ld]` file.txt'`  -- 从 file.txt 中查找含有 gl 或 gd 字符串的行，并列出行号
[n1-n2]  | 字符只能在 n1～n2 连续区间内 | `grep -n '[0-9]' file.txt'`  -- 从 file.txt 中查找含有任意数字的一行，并列出行号
[^list]  | 字符不能包含 list 集合中的任一字符 | `grep -n oo[^t] file.txt'`  -- 从 file.txt 中查找包括 oo 但不能是 oot 这样的字符串行，并列出行号
c\{m,n\}  | 连续 m 到 n 个前一个字符  | `grep -n 'go\{2,3\}g' file.txt'` -- 从 file.txt 中查找包含 goog 或者 gooog 的行，并列出行号
