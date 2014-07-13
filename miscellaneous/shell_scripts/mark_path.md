# 快速访问常用目录

有时候我们在命令行下经常访问一些常用的目录，如果这些目录层次很深，每次都 `cd` 进去，未免很麻烦，下面这方法可以有效解决这个问题：

编辑 /etc/profile 添加下面的代码：

``` bash
# mark
export MARKPATH=$HOME/.marks
export MARKDEFAULT=sanguo#设置你的默认书签，可以直接输入g跳转
 
function g {
    local m=$1
    if [ "$m" = "" ]; then m=$MARKDEFAULT; fi
    cd -P "$MARKPATH/$m" 2>/dev/null || echo "No such mark: $m"
}
function mark {
    mkdir -p "$MARKPATH"
    local m=$1
    if [ "$m" = "" ]; then m=$MARKDEFAULT; fi
    rm -f "$MARKPATH/$m"
    ln -s "$(pwd)" "$MARKPATH/$m"
}
function unmark {
    local m=$1
    if [ "$m" = "" ]; then m=$MARKDEFAULT; fi
    rm -i "$MARKPATH/$m"
}
function gs {
    ls -l "$MARKPATH" | grep ^l | cut -d ' ' -f 13-
}
 
_completemarks() {
    local curw=${COMP_WORDS[COMP_CWORD]}
    local wordlist=$(ls -l "$MARKPATH" | grep ^l | cut -d ' ' -f 13)
    COMPREPLY=($(compgen -W '${wordlist[@]}' -- "$curw"))
    return 0
}

complete -F _completemarks g unmark
```

进入我们需要访问的目录，例如这里我经常要进入一个项目的根目录，那么我可以给这个目录加一个“标签”一样的标记，下次直接跳到这个“标签”所标记的目录就好了：

```
cd /home/hailin.ghl/workspace/sls_011_trunk/sls/ilogtail
mark logtail   #mark this dir as logtail

g logtail   #use this command, we can go to the upper dir quickly

umark logtail   #delete the mark
```

----
方法出自： http://www.ccvita.com/520.html
