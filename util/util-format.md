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

    const formatRegExp = /%[sdj%]/g;   //匹配 %s, %d, %j 
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
    var str = String(f).replace(formatRegExp, function(x) {
    if (x === '%%') return '%';
    if (i >= len) return x;
    switch (x) {
      case '%s': return String(args[i++]);
      case '%d': return Number(args[i++]);
      case '%j':
        try {
                return JSON.stringify(args[i++]);
            } catch (_) {
              return '[Circular]';
            }
        // falls through
        default:
            return x;
        }
      });
      for (var x = args[i]; i < len; x = args[++i]) {
        if (x === null || (typeof x !== 'object' && typeof x !== 'symbol')) {
          str += ' ' + x;
        } else {
          str += ' ' + inspect(x);
        }
      }
      return str;
    };
