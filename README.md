# nodejs-analyse
analyse the origin code of nodejs

#Part1 : util api
##util.format(format[,...])
通过在第一个参数使用类似于printf格式，返回一个格式化以后的字符串。
第一个参数是一个字符串，它包含零个或多个占位符。每一个占位符都被相应的参数转换以后的值所代替。
被支持的占位符有：
-%s - 字符串
-%d - 数字（正数或浮点数）
-%j - JSON.如果参数含有循环引用，则它将被'[圆形]'字符串代替。
-%% - 单个百分号('%').这个符号不消耗参数。

如果占位符没有相对应的参数，则占位符将不会被取代。

    util.format('%s:%s','foo');  // 'foo:%s'

