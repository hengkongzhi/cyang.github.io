## gprof、gcov
### gprof
+ gprof能够打印出程序运行中各个函数消耗的时间，可以产生程序运行时候的函数调用关系，包括调用次数，可以帮助程序员分析程序的运行流程  
+ 使用 **`-pg`** 选项编译和链接应用程序，例如在gcc编译程序的时候，加上 **`-pg`** 选项：  
```shell
    gcc -pg -o test test.c
```
+ 执行应用程序使之生成供gprof分析的数据，例如生成gmon.out文件：  
```shell
    ./test
```
+ 使用gprof命令分析应用程序生成的数据gmon.out： 
```shell 
    gprof test gmon.out > parse.txt
```
+ 使用gprof2dot.py对gprof分析文件生成dot文件  
```shell
   python gprof2dot.py -n0.1 -e0.1 parse.txt -o 1.dot
   #-n0.1和-e0.1表示节点时间和调用时间耗用占比小于%0.1的不显示
```
+ 安装graphviz，使用dot命令将dot文件转换为pdf文件  
```shell
   dot -Tpdf 1.dot -o 1.pdf
```

### gcov
+ 利用gcov工具能够检测测试用例对源代码的覆盖率，可以较好的检查到代码的所有逻辑是否覆盖  
+ 要开启gcov功能，需要在源码编译参数中加入 **`-fprofile-arcs -ftest-coverage`** ：  
```shell
    gcc -fprofile-arcs -ftest-coverage -o test test.c
    #编译完成后，会生成test.gcno文件
```
+ 执行应用程序  
```shell
   ./test
   #执行完成后，会生成test.gcda文件
```
+ 使用gcov命令分析应用程序执行后的代码覆盖率  
```shell
    gcov -r test.c
    #将展示test.c的覆盖率，并生成test.c.gcov文件，可直接cat查看该文件，每行行首的数字表示该代码执行的次数，-:表示未执行过
    #-r选项表示，只生成程序对应的源码文件的覆盖率报告，其余系统头文件和内联函数文件不生成覆盖率报告
```
+ 改变测试数据，多次执行应用程序和gcov命令生成代码的覆盖率报告  
