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

    const formatRegExp = /%[sdj%]/g;   //匹配 %s, %d, %j, %%
    exports.format = function(f) {    //此处的f表示第一个参数
    if (typeof f !== 'string') {
        var objects = [];
        for (var i = 0; i < arguments.length; i++) {
          objects.push(inspect(arguments[i]));
        }
        return objects.join(' ');       //如果第一个参数不是字符串，则对每个参数进行util.inspect()处理之后，再将它们拼接起来返回
      }
        //以下的情况均是在第一个参数是字符串的前提下进行
    if (arguments.length === 1) return f;  //如果参数的长度为1,则直接将其返回
    var i = 1;
    var args = arguments;
    var len = args.length;
        /****字符转换，核心内容***/
    var str = String(f).replace(formatRegExp, function(x) {  //此处函数中x表示的是matched的内容;
    if (x === '%%') return '%';                              //如果匹配的结果为%%,则返回%
    if (i >= len) return x;                                  //replace中的replacer为此处的function(x)，因为replacer函数可能会执行多次，导致i自加，所以i可是2,3...，如果匹配的占位符的数量大于参数的数量，则直接将占位符返回。
    switch (x) {
        case '%s': return String(args[i++]);                   //匹配到%s,将第二个参数转化为string ,并对i自加1  
        case '%d': return Number(args[i++]);                   //匹配到%d,将第二个参数转化为number ,并对i自加1
        case '%j':                                             //匹配到%j,
            try {
                    return JSON.stringify(args[i++]);           //
                } catch (_) {
                    return '[Circular]';                        //？？ 循环引用 ？？
                }
        // falls through
        default:
            return x;                                           //其他情况，例如没有匹配的东西('')，则不做替换
        }
      });
      /****字符转换结束***/
      for (var x = args[i]; i < len; x = args[++i]) {           //此处i的初值是没有相应占位符的剩下的第一个参数
        if (x === null || (typeof x !== 'object' && typeof x !== 'symbol')) {  //如果没有剩下的参数，或者剩下的参数既不是object类型也不是symbol类型的话，就将参数直接用' '追加到str后面
          str += ' ' + x;
        } else {
          str += ' ' + inspect(x);                              //否则将剩下的参数用util.inspect()方法处理以后，用' '隔开并追加到str后面
        }
      }
      return str;
    };
注：
- 字符串replace方法：[String.prototype.replace(regexp,function)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace#Specifying_a_string_as_a_parameter)
- ES6中的第七种类型--symbol。[中文CSDN地址](http://www.csdn.net/article/2015-07-09/2825172-es6-in-depth-symbols)
      [英文地址](https://hacks.mozilla.org/2015/06/es6-in-depth-symbols/)

