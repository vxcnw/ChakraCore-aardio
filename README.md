# ChakraCore-bindings-for-aardio
#### [ChakraCore](https://github.com/chakra-core/ChakraCore) is a Javascript engine with a C API you can use to add support for Javascript to any C or C compatible project. 
#### [aardio](http://www.aardio.com/)  is an extremely easy-to-use dynamic language, but it is also a hybrid language that allows for rare and very convenient manipulation of static types, so it can directly call API interface functions of static languages such as C, C++, etc.

### A simple example
***
```javascript
io.open()
// https://github.com/chakra-core/ChakraCore/wiki/Embedding-ChakraCore

import aaz.libchakraCore;

// 创建一个新的运行时
var runtime = aaz.libchakraCore.createRuntime();
// 创建用于运行脚本的脚本上下文
var context = runtime.createContext();
// 在线程上设置当前脚本上下文
aaz.libchakraCore.context.current = context;

// 运行脚本
var jsScript = "(()=>{return 'Hello world!';})()"
var jsValue, err = aaz.libchakraCore.context.runScript(jsScript, 1, "");
if(!jsValue){
	io.print("脚本错误：" ++ err)
	execute("pause")
}

// 使用 jsValue 前增加引用次数，防止中间被垃圾回收。 注：使用已被垃圾回收的 jsValue 会导致程序崩溃
jsValue.addRef();

// 把 js 字符串转为 aardio 字符串

// 方法一， 提前知道 jsValue 是字符串
var result = jsValue.toString();
io.print(result)

// 方法二 ，使用辅助函数，检测 jsValue类型后然后转换为 aardio 内置的数据类型
var result = jsValue.parse();
io.print(result)

// 使用 jsValue 完毕后要减少引用次数
jsValue.release();



// 测试创建一个 jsValue ， 注意创建前要有一个活动的上下文，这里使用上面已创建的
var testValue = aaz.libchakraCore.value("中a1");
testValue.addRef();

// 读取里面的内容
io.print( testValue.parse() )
testValue.release();

// 释放资源
// 在线程上删除当前上下文
aaz.libchakraCore.context.current = null;
// 释放运行时
runtime.dispose();

execute("pause")
```
