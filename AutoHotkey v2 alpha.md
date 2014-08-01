翻译[AutoHotkey v2 alpha](http://ahkscript.org/boards/viewtopic.php?f=24&t=2120)

---

关于AHK V2，见[链接](http://ahkscript.org/v2/)。每次的变更记录会追加到主题。

##[V1.1自V2.0-a的变更文档](http://ahkscript.org/v2/v2-changes.htm)

###v2.0-a045-bbf6f99

操作符`is`替换`if var is typetype`（类型）是字符串，故需置于引号中（或变量中），如，`n is 'number'`。支持的类型名称新增`object`、`byref`。此外，对`x is y`，当`y`是对象时，检测`x`是否派生自`y`，不论直接或间接。

`contains`及`in`为脚本保留的关键词。

[V2变更文档](http://ahkscript.org/v2/v2-changes.htm)中*Error Handing（错误处理）*，需强调：

* 在`try`块内使用命令，不再致使异常。
* `DllCall`与`RegExMatch/Replace`使用异常取代`ErrorLevel`。
* 表达式求值与对象使用中包含更多可能触发异常的条件检查。
* `OnMessage`失败时抛出异常；若无消息处理函数，返回空字符串。

之前v2 alphas版本中`Commands()`返回`ErrorLevel`的情况，如今返回`!ErrorLevel`（1-成功，0-失败），除了`RunWait`与`SendMessage`，他们仍返回`ErrorLevel`。不论命令以何种方式调用，命令均会设置`ErrorLevel`。见[V2变更文档](http://ahkscript.org/v2/v2-changes.htm)中*Command()（命令函数调用形式）*部分。

修复内嵌函数的输出变量，以便在命令形式的调用中无需添加百分号。

所以的`WinSet`函数成功时返回1，失败时返回0，同时，`ErrorLevel`成功时为0，失败时为1。

`word ? x : y`、`word ++`与`word --`不再是表达式，因为`word`可能为用户定义的命令。要使用三元操作符或后置自增操作符或后置自减操作符，可移除变量与操作符间的空格，或，使用圆括号包裹变量。

移除`ComObjMissing()`。使用两个连续逗号取代，如`x.y(a, , c)`或`p := [a, , c], x.y(p*)`。

修正`x[1,2]+=y`与`%i-1%`。

---

##v2.0-a046-692ef59

移除`ComObj...()`的"多态"行为。  
从`ComObject()`中移出`ComObjActive()`，并改变"持有者关系" - 见后。  
移除`ComObjUnwrap(obj)`与`ComObjTypo(obj,...)`。  
改变正则选项``a`，使`\R`可匹配Unicode新行符。  
禁止`OnMessage`中使用方法做消息响应函数，以避免混乱。  
恢复`#Persistent`指令。  
完善浮点数格式的字符串，提升精度并避免被截短。  
修正那些不涉及`ErrorLevel`的命令，不再返回1。  
改变`FileOpen()`的错误处理方式至与其他命令一致。  

重命名`Args`为`A_Args`，并略微改变其行为。

* 如果脚本无参数，则赋值空数组，进而`A_Args[1]`不会致使错误。
* 作为超级全局变量，如同其他内置变量。

`Hotkey`所设置的热键不再默认视为如同存于脚本最底端`#If`/`#IfWin`之后。  
所设置的热键/热字串默认与当前热键的条件相同，故`Hotkey, %A_ThisHotkey%, Off`将关闭当前热键，即便它是上下文相关的。  
其他线程默认为最近一次于自动执行段处的设置，默认为无限定条件（即全局热键）。  

`ComObject(pdsp)`、`ComObject(9, pdsp)`与`ComObject(13, pdsp)`默认不再调用`AddRef`；默认他们将作为指针的"持有者"。  
多数V1脚本中的`ComObjEnwrap(pdsp)`（摘自Google搜索中的前几页）是使用错误的；如，他们未释放自身对指针的引用副本。  
V2中，若无别处"持有"关于`pdsp`的引用，则脚本须在`ComObject(pdsp)`前调用`ObjAddRef(pdsp)`（因为，当代理对象被释放时，或，作为`ComObject()`内部查询IDispatch时的副作用——即刻地，指针会被自动释放）。  
`flags`参数当前仅作用于`SafeArrays`。  
