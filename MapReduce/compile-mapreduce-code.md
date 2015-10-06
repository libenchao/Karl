# 编译MapReduce程序
1. 首先进行编译

        javac -encoding utf-8 -classpath /root/hadoop-0.21.0/hadoop-core-1.0.0.jar:/root/hadoop-0.21.0/lib/commons-cli-1.2.jar:/root/hadoop-0.21.0/lib/commons-logging-1.1.1.jar -d classes  src/*/*.java src/*/*/*.java  
    * 这里需要添加几个依赖包，一般情况下有以上三个就够了。
    * 然后是需要指定输出目录，用**-d**选项
    * 再然后就是指定源文件，这里需要注意的是，要指定到具体的java文件，而不能只是指定一个目录。可以用*通配符
2. 然后进行打包

        jar -cvf reduction.jar -C classes/ .
    * reduction.jar 是输出文件的名字
    * -C选项指定输入文件夹。不要忘记最后的 **'.'**
3. 最后运行
    * 最后运行的话就是直接用hadoop jar命令运行就行了。
    * 但是不要忘记了java源代码中如果有package的话，需要指定绝对路径。