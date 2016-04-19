###翻译自:https://nodejs.org/dist/latest-v5.x/docs/api/util.html#util_util_inspect_object_options
###源码地址：https://github.com/shixiaomiaomiao/node/blob/master/lib/util.js

#util.inspect(object[,options])
返回object对象的字符串形式，这对于调试时有用的。
一个可选的options对象也许可以被传递来改变格式化后的字符串的某些特定的方面。
- showHidden -默认值为false;如果改为true,则object对象的不可数属性和符号属性也会显示出来。
- depth -默认值为2。该值告诉inspect方法，在格式化object对象时应该递归多少次。该值对于检查非常复杂的objects对象时时非常有用的。不确定要递归多少次，则传入null。
- colors -默认值为false。如果设置为true,则输出值将会呈现ANSI颜色代码。颜色是可定制的，查看[如何定制util.inspect颜色](#customizing)
- customInspect -默认值为true。如果设置为false,则定义在被检查的objects对象上的 定制inspect(depth,opts)函数将不会被调用。

检查util的object对象的所有属性的例子：

    const util = require('util');
    console.log(util.inspect(util, { showHidden : true, depth : null}))

values可以提供他们自己定制的inspect(depth, opts)函数，在递归检查中调用这些函数时将会收到当前的depth，以及被传入util.inspect()中的optons object对象。

<a href = '#customizing' id = 'customizing'></a>
#定制util.inspect颜色
util.inspect输出值的颜色（在被允许的情况下）是通过util.inspect.styles和util.inspect.colors对象在全局定制的。
util.inspect.styles是一个map，它从util.inspect.colors对象上分配到每一个颜色风格。

突出显示和它们的默认值是：
- number : yellow
- boolean : yellow
- string : green
- date : magenta(品红色)
- regexp : red
- null : bold (粗体)
- undefined : grey (灰色)
- special(目前只是函数) : cyan (蓝绿色) 函数名字(故意没有风格)

预先设定的代码颜色有white, grey, black, blue, cyan, green, magenta, red 和yellow.还有bold(粗体), italic(斜体),underline(下划线)和inverse(倒转的)代码。

#在对象上定制inspect()函数
对象也可以定义它们自己的inspect(depth)函数，
