## python语法错误识别的ace编辑器

#### 起源

代码fork from [PythonSyntaxValidator](https://github.com/ethanchewy/PythonSyntaxValidator)，但该项目里的`ace-editor`代码是2014年的，太过于久远。

因此在此基础上，基于[ace-builds](https://github.com/ajaxorg/ace-builds/tree/7489e42c81725cd58d969478ddf9b2e8fd6e8aef) `v1.4.5`，版本号`7489e42`进行改造。

#### python语法错误识别的原理

在worker-python中，构建了js版本的python编译器、解析器等，当ace里的代码被编译后，如果被catch捕获，则执行ace的setAnnotations，展示ui上的错误提示

```
var prsr = require('../mode/python/Parser');
var bldr = require('../mode/python/builder');
var cmplr = require('../mode/python/compiler');
var symtbl = require('../mode/python/symtable');
try  {
  var fileName = '<stdin>';

  var node = prsr.parse(fileName, source);

  var module = bldr.astFromParse(node, fileName);

  var symbolTable = symtbl.symbolTable(module, fileName);

  var compiler = new cmplr.Compiler(fileName, symbolTable, 0, source);

  var compiled = { 'funcname': compiler.cmod(module), 'code': compiler.result.join('') };
} catch (e) {
  try  {
    annotations.push({
      row: e.lineNumber - 1,
      column: e.columnNumber,
      text: e.message,
      type: ERROR
    });
  } catch (slippery) {
    console.log(slippery);
  }
}
```
