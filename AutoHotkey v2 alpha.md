翻译[AutoHotkey v2 alpha](http://ahkscript.org/boards/viewtopic.php?f=24&t=2120)

---

关于AHK V2，见[链接](http://ahkscript.org/v2/)。每次的变更记录会追加到主题。

##[V1.1自V2.0-a的变更文档](http://ahkscript.org/v2/v2-changes.htm)

###v2.0-a045-bbf6f99

操作符`is`替换`if var is typetype`（类型）是字符串，故需置于引号中（或变量中）—如，`n is 'number'`。支持的类型名称新增`object`、`byref`。此外，对`x is y`，当`y`是对象时，检测`x`是否派生自`y`，不论直接或间接。

`contains`及`in`为脚本保留的关键词。

[V2变更文档](http://ahkscript.org/v2/v2-changes.htm)中*Error Handing（错误处理）*，需强调：

* 在try块内使用命令，不再致使异常。
* DllCall与RegExMatch/Replace使用异常取代ErrorLevel。
* 表达式求值与对象使用中包含更多可能触发异常的条件检查。
* OnMessage失败时抛出异常；若无消息处理函数，返回空字符串。

之前v2 alphas版本中`Commands()`返回`ErrorLevel`的情况，如今返回`!ErrorLevel`（1-成功，0-失败），除了`RunWait`与`SendMessage`，他们仍返回`ErrorLevel`。不论命令以何种方式调用，命令均会设置`ErrorLevel`。见[V2变更文档](http://ahkscript.org/v2/v2-changes.htm)中*Command()*部分。

修复内嵌函数的输出变量，以便在命令形式的调用中无需添加百分号。

所以的WinSet函数成功时返回1，失败时返回0，同时，ErrorLevel成功时为0，失败时为1。

`word ? x : y`、`word ++`与`word --`不再是表达式，因为`word`可能为用户定义的命令。要使用三元操作符或后置自增操作符或后置自减操作符，可移除变量与操作符间的空格，或，使用圆括号包裹变量。

移除ComObjMissing()。使用两个连续逗号取代，如`x.y(a, , c)`或`p := [a, , c], x.y(p*)`。

修正`x[1,2]+=y`与`%i-1%`。


