# 简单、易用的 bash 脚本

## 统计文件中单词的个数

``` bash
tr -c '[:alnum:]' '\n' | grep -v '^[0-9]*$' | sort | uniq -c | sort -r
```

* `tr -c '[:alnum:]' '\n'`
* `grep -v '^[0-9]*$'`
* `sort`
* `uniq -c`
* `sort -r`

