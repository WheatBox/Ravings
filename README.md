# 胡言乱语 Ravings

## 介绍

你说的对，但是《胡言乱语 Ravings》是由 Github@WheatBox 专为 GML 开发的一款可在游戏运行时无需编译即可直接运行的脚本语言（准确点来说，是它的解释器），主要用于 Mod 系统、热更新 等要在运行时增加新内容的情况。

该脚本语言适用于一个被称作「GameMaker」的游戏引擎，在这里，被游戏开发者选中的游戏将被授予「胡言乱语解释器」，引导 Mod 系统之力。你将扮演一位名为「第三方创作者」的神秘角色，在自由的支持 Ravings 的游戏中制作形式多样、功能独特的 Mod，用以扩充游戏内容，增加游戏乐趣——同时，逐步发掘「胡言乱语 Ravings」的真相。

胡言乱语 Ravings 的语法主要参照了 JS，甚至可以说就是山寨出来了个阉割版的 JS 来给 GM 的游戏在运行时用的，所以会 GM 或 JS 的开发者很容易就能上手

## 进度

### GameMaker 版

搁在一边，等 JS 版做完了直接复制过来改改数组、字符串、Map之类的数据结构和相关函数就能用了

至于为啥做了 JS 版，因为 VSCode 写代码体验比 GameMaker 内置的代码编辑器要舒服太多了，而且用 Node.js 或浏览器来进行调试更方便一些

### JavaScript 版

- [x] 运算能力
- [x] 关键字 var 和 赋值
- [x] 关键字 if
- [x] 关键字 else
- [x] 关键字 while
- [x] 关键字 for
- [x] 关键字 break
- [x] 关键字 continue
- [ ] 关键字 function 和 函数调用
- [ ] 关键字 return
- [x] 创建数组 []
- [x] 访问数组 []
- [ ] 创建对象 {}
- [ ] 访问对象 .
- [x] 字符串 ""
- [ ] lambda 函数
- [ ] 注释 // /* */

## 保留关键字

    var 定义变量
    function 定义函数
    if 分歧语句
    else 分歧语句
    for 循环语句
    while 循环语句
    break 终止循环
    continue 跳过一次循环
    return 返回

## 开发中一些问题的解决方案

### 关于每一个元素的格式

每个元素都会以数组的形式存储，[0] = EType.xxx，[1] = 本体

### 关于分段代码

循环遍历每一个符号

检测是否为空格，若是，则跳过

检测是否为单字符的运算符（, . ( ) 等），若是则分段，并继续去执行后续分段检测与处理

检测是否为双字符的运算符的第一个字符（* + - / = | & 等），若是则再判断下一个字符是否与当前字符可构成双字符运算符，若是则分段于一起，若不是则仅分段第一个字符，并去从第二个字符开始再做分段检测与处理

检测是否为数字（0 1 2 3 4 等），若是则继续往后判断字符是否为数字，直至判断到非数字再分段

若以上都不是，则会视为标识符，并继续往后检测字符，直至判断到数字或特殊符号再分段，注意此处分段时会顺便判断是否为保留关键字，若为保留关键字则再做进一步处理

还有一些对于特定字符或关键字做出的特殊处理，例如 ; { } if 等，详情请见 Ravings.CutCode()

### 关于作用域

Ravings.arrVarMaps 是一个 装有 装有 用户定义的rvs变量 的Map 的数组

每一个下标对应着一个层级，层级以函数来进行划分，当执行一个函数时建立新层级，函数结束时时删除最新的层级
