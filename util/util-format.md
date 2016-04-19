#util.format(format[,...])
通过在第一个参数使用类似于printf格式，返回一个格式化以后的字符串。
第一个参数是一个字符串，它包含零个或多个占位符。每一个占位符都被相应的参数转换以后的值所代替。
被支持的占位符有：

- %s - 字符串
- %d - 数字（正数或浮点数）
- %j - JSON.如果参数含有循环引用，则它将被'[圆形]'字符串代替。
- %% - 单个百分号('%').这个符号不消耗参数。

如果占位符没有相对应的参数，则占位符将不会被取代。

    util.format('%s:%s','foo');  // 'foo:%s'
如果传入的参数多于占位符，则多余的参数将被转化为字符串（对于objects对象和符号，util.inspect()被使用），然后被拼接在一起，用空格隔开。

    util.format('%s:%s', 'foo', 'bar', 'baz');   // 'foo:bar baz'
    
如果第一个参数不是一个格式字符串，则util.format()返回一个字符串，该字符串由所有的参数用空格分隔后拼接而成。每个参数都用util.inspect()方法转化成一个字符串。

        var object = {'1':2};
        var string = 'shijiale';
        var number = 456;
        util.format ( object, string, number );  // '{\'1\':2} \'shijiale\' 456'

###源代码
