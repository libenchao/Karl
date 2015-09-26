# utf-8编码问题

python源码文件中，如果没有指定具体的编码格式，则使用默认的ASCII编码。对于很多需要处理中文的程序来说，很不方便，如果要自己指定文件的编码方式，可以在源码文件的**开头**指定，具体指定方式如下：

1. 开头直接写

        # coding=<encoding name>
2. 自动执行脚本中，第二行写

         #!/usr/bin/python
         # -*- coding: <encoding name> -*-
3. 使用vim的设定方式

        #!/usr/bin/python
        # vim: set fileencoding=<encoding name> :


其实就是要在源码的第一行或者第二行满足***`coding[:=]\s*([-\w.]+)`***这个正则表达式  
具体参考：[PEP 0263](https://www.python.org/dev/peps/pep-0263/)

<2015年9月26日>