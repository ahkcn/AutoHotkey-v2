翻译[AutoHotkey v2 alpha](http://ahkscript.org/boards/viewtopic.php?f=24&t=2120)

---

关于AHK V2，见[链接](http://ahkscript.org/v2/)。每次的变更记录会追加到主题。

##[V1.1自V2.0-a的变更文档](http://ahkscript.org/v2/v2-changes.htm)

###v2.0-a045-bbf6f99

操作符`is`替换掉`if var is type`。type（类型）的值是字符串，故需置于引号中（或变量中），如，`n is 'number'`。支持的类型名称新增`object`、`byref`。此外，对`x is y`，当`y`是对象时，检测`x`是否派生自`y`，不论直接或间接。

`contains`及`in`为脚本保留的关键词。

[V2变更文档](http://ahkscript.org/v2/v2-changes.htm)中*Error Handing（错误处理）*，需强调：

* 在`try`块内使用命令，不再致使异常。
* `DllCall`与`RegExMatch/Replace`使用*异常*取代`ErrorLevel`。
* 表达式求值与对象使用中包含更多可能触发*异常*的条件检查。
* `OnMessage`失败时抛出*异常*；若无消息处理函数，返回空字符串。

之前v2 alphas版本中`Commands()`返回`ErrorLevel`的情况，如今返回`!ErrorLevel`（1-成功，0-失败）；除`RunWait`与`SendMessage`外，他们仍返回`ErrorLevel`。不论命令以何种方式调用，命令均会设置`ErrorLevel`。见[V2变更文档](http://ahkscript.org/v2/v2-changes.htm)中*Command()（命令的函数调用方式）*部分。

修复内置函数的输出变量，以便在命令调用方式中无需再为其添加百分号。

所有的`WinSet`函数成功时返回1，失败时返回0，同时，`ErrorLevel`成功时为0，失败时为1。

`word ? x : y`、`word ++`与`word --`不再是表达式，因为`word`可能为用户定义的命令。要使用三元操作符或后置自增操作符或后置自减操作符，可移除变量与操作符间的空格，或，使用圆括号包裹变量。

移除`ComObjMissing()`。使用两个连续逗号取代，如`x.y(a, , c)`或`p := [a, , c], x.y(p*)`。

修正`x[1,2]+=y`与`%i-1%`。

---

###[v2.0-a046-692ef59](http://ahkscript.org/boards/viewtopic.php?p=16591#p16591)

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


---
###[v2.0-a047-3defc4a.txt](http://ahkscript.org/boards/viewtopic.php?p=17284#p17284)

修正`co`、`con`、`contain`等，不作为表达式的保留字。  

合并v1.1.14.04。

---
###[v2.0-a048-573f142](http://ahkscript.org/boards/viewtopic.php?p=20021#p20021)

增强Click，使支持函数语法 - `Click(x, y)`。	
改变，`RegExMatch` / `RegExReplace`在PCRE执行错误时，抛出异常。	
改变，&(var:=1)返回变量的64位数值缓存，而非字符串缓存。	
改变，对象处理数值字符串为字符串，而非数值。

---
###[v2.0-a049-ea663d0](http://ahkscript.org/boards/viewtopic.php?p=22371#p22371)

`Objects`（对象）与`SafeArrays`的内置方法不再支持_（下划线）前缀，除了`_NewEnum`（因为COM对象的兼容性需要）。`ObjNewEnum(x)`仍等效`x._NewEnum()`。

为了解决关于对象双重性的混乱（如，关联数组可作线性数组使用），方法`Insert`与`Remove`拆分如下（其中`Object`、`Array`及`Stack`均为对象）：


* `Value := Object.Remove(Key)`：移除Key（键），返回他对应的值。
* `Count := Object.Remove(FirstKey, LastKey)`：移除关于键的范围，返回实际移除的键值对的数量。与v1不同，不影响其他键。
* `Array.InsertAt(Index, Value1, ...)`：在指定位置插入一个或多个值。
* `Value := Array.RemoveAt(Index)`：移除并返回数组索引（Index）处的元素。
* `Count := Array.RemoveAt(Index, vCount)`：移除所有自索引（Index）至索引+vCount-1范围内的数组元素。返回值Count（数量）为数组中实际存在的元素。
* `Stack.Push(Value1, ...)`：在数组末尾插入一个或多个值。
* `Value := Stack.Pop()`：移除并返回最后一个数组（栈）元素。
* 仍存在等效于内置函数的方法；如，`ObjPop(Stack)`。
* `ObjRawSet(Object, Key, Value)`：赋值到对象，跳过任何元方法。

`Object.MaxIndex()`变为`Object.Length()`（或`Object.Length`），当无整数键时返回0。 

移除`Object.MinIndex()`，较MaxIndex其极少使用。可用for-loop（循环）查找最小键。

对象方法中的参数数量检测及内存耗尽响应按照其他函数那样处理。（将在运行时提示错误）

修正使用命令语法传递对象与数字到函数的错误。

支持使用命令语法调用方法：

* 命令名处，使用对象与句点连接的方法。`Var.Method`及`Var.Property.Method`均可，但更复杂的情况（如 `Var[x].Method`或`%Var%.Method`）不可。
* 返回值被丢弃；没有输出变量。
* 所有的参数默认为文本字符串。

---
### [v2.0-a050-712e1d8](http://ahkscript.org/boards/viewtopic.php?p=26040#p26040) ###

概要：涉及多个代码区域，Bug及潜在Bug修复、优化。

增加支持：表达式及对象中的字符串可包含二进制零。  
增加支持：可使用`==`比较二进制数据。  

[fincs]更改[OnExit](http://lexikos.github.io/v2/docs/commands/OnExit.htm)使可接收函数参数。  
[fincs]移除`OnClipboardChange`，增加[类似于OnExit的函数](http://lexikos.github.io/v2/docs/misc/Clipboard.htm#OnClipboardChange)。  
更改`ExpandExpression()`，使在（调用？）完成时释放对象引用。

优化函数返回的动态分配的值。  
修正`GetKeyState`及`KeyWait`。

* 移除ACT\_GETKEYSTATE，减小代码大小，并修复`GetKeyState Joy%btn%`以返回0/1而非U/D。
* 修复`JoyPOV`当居中时返回整数-1，而非字符串。
* 更改`KeyWait`，当`KeyName`无效时将抛出错误。

更改命令/函数返回纯整数的HWND及（或?）style。涉及如下：

* WinActive
* WinExist
* WinGetID
* WinGetIDLast
* WinGetList
* WinGetStyle
* WinGetStyleEx
* WinGetControlsHwnd
* ControlGet Hwnd
* Gui +HwndVarname
* GuiControlGet Hwnd
* Gui Add, ..., +HwndVarname
* MouseGetPos

由于技术限制，`A_ScriptHwnd`仍返回字符串，考虑一致性，为十进制数值形式。  
更改`Func()`使可接收函数名或函数引用。  
移除`Object()`的*对象->地址*模式。使用`ObjAddRef(addr := &object)`代替。  
更改`ExitApp`，可以传递退出代码（`ExitCode`）至`OnExit`回调。  
更改`OnClipboardChange()`可使脚本持续化。  
更改`OnClipboardChange()`，使其不立即调用函数。

更改`StrSplit()`在无效分隔符时抛出错误。  
更改`VarSetCapacity`在容量无效/过大时抛出错误。  
更改`File`命令在无效参数时抛出错误。

修正`X(,(y))`与`X(,"%y%")`不正确地导致错误的问题。  
修正`return "string"`（返回字符串）至`CallFunc()`时导致崩溃的问题。  
修正`return "字符串"`（返回"带引号的字符串"）的结果不包含引号的问题。  
修正关于ACT\_METHOD的简单`% int`（整数）参数。（？）  
修正优化，不再致使`A_Index`/`A_EventInfo`变为非数值。  
修正`x.y`作为调用，而非读，与`x.y,`一致。旁注：原本就禁止单独的`x.y`表达式。此项更改用于支持惰性VB转换，以便使用类似`App.Quit`形式的代码，其应该仍可以工作，且当前支持对象了。  
修正，将`x.1`作为关于数值键的调用，与`x.1()`一致。  
修正方法做命令调用，当需要时会触发默认基对象。  
修正`StrPut("",Encoding)`-修正于c3d844e，后于855815e被破坏。  
修正ACT\_FUNC/ACT\_METHOD以支持`% v:=1`及`% v:={}`输入形式的引用。  
修正`throw str`（抛出字符串）。  
修正`Abs()`/`Mod()`不再覆盖（字符串/对象）参数。  
修正ACT\_FUNC/ACT\_METHOD中的纯文本参数（于69e9675被损坏）。  
修正一些对象引用泄露。  
修正无参数的命令形式调用，不再需要输出参数。  
修正当`RegExReplace`抛出异常时的内存泄露。  
修正`RegEx`调出，使其可以抛出异常或退出线程。  
修正一些在对象中使用字符串的错误（于8cd2072破坏）。  
修正`&byref_var_containing_int`（对包含数值的引用变量取地址）。

合并v1.1.16.03（及对错误的修复）。
