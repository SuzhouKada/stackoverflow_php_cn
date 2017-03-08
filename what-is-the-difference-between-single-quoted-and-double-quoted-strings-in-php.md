原文地址

http://www.stackoverflow.com/questions/3446216/what-is-the-difference-between-single-quoted-and-double-quoted-strings-in-php

###在PHP中，单引号字符串和双引号字符串之间有什么区别？

我刚接触PHP编程，我发现有的PHP字符串在单引号里，有的则在双引号里，这样让我很困惑。在此之前，我只了解.Net和C语言，在它们里面单引号表示字符而不是字符串。

## 回答

在PHP中，字符串可以通过4种方式指定：

*1.单引号*

单引号表示的字符串会显示字符串本身，其中的变量和大多数的转义序列不会被解释。会被解释的特例是：要想表示单引号本身，那么需要用\，比如\'，表示反斜杠本身需要使用\\来表示。

*2.双引号*

双引号可以解释包括正则表达式在内的转义字符，其中包含的变量也会被解析。很重要的一点是PHP在$之后组合尽量多的标识以形成一个合法的变量名，你可以用花括号来明确变量名的界线。例如你现在有变量$type，但是有如下代码

<pre>
echo "The $types are"
</pre>

它会寻找变量$types来解析，在这里就可以使用花括号来明确你想要的变量是$tyoe

<pre>
echo "The {$type}s are"
</pre>

此处的{可以放在$之前或者之后（但必须紧挨着它），关于更深入的变量解析，如使用数组可以在下面网址查阅有关变量解析的文档。

> http://php.net/manual/zh/language.types.string.php#language.types.string.parsing

*3.HereDoc*

HereDoc语法类似于双引号字符串的语法。它要以<<<开始，并在其后提供一个标识符并换行。接下来写字符串本身的内容，最后以之前的标识符作为结束标志。在这种表达方式中，不需要转义符。
以下为在php.net找到的示例

<pre>
<?php
class foo
{
    var $foo;
    var $bar;

    function foo()
    {
        $this->foo = 'Foo';
        $this->bar = array('Bar1', 'Bar2', 'Bar3');
    }
}

$foo = new foo();
$name = 'MyName';

echo <<<EOT
My name is "$name". I am printing some $foo->foo.
Now, I am printing some {$foo->bar[1]}.
This should print a capital 'A': \x41
EOT;
?>
</pre>

输出结果如下：

<pre>
My name is "MyName". I am printing some Foo.
Now, I am printing some Bar2.
This should print a capital 'A': A
</pre>

*4.NowDoc*

NowDoc在语法上就类似单引号（PHP 5.3.0之后）。区别在于不需要转义'或者\。它的开始标志也是<<<，与HereDoc 相同，但是后面的标识符用单引号括起来，比如<<<'EOT'。在NowDoc中不进行解析。

*关于速度：*

关于它们速度的对比，这里有一篇文章介绍了在PHP 4.3以后从本质上他们是一样快的。

> http://phplens.com/lens/php-book/optimizing-debugging-php.php

另外在下面网址有关于单引号和双引号的速度对比，大部分是相同的，只有一个对比双引号比单引号慢。

> http://www.phpbench.com/
