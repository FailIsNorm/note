## go语言环境搭建

------

##安装go开发包

cmd： go version 确认版本号

------

bin：存放编译后的二进制文件

Pkg: 存放编译后的库文件

src：存放工程文件

------

## 执行go脚本

------

### 编译

go build 

win系统下会生成.exe文件 直接在集成terminal执行即可

mac下会生成可执行二进制文件

go build -o name.exe

生成一个可执行的文件（name.exe ）

### 执行 

go run main.go

执行主函数文件                                              

------

go install 

先执行go build命令 然后将文件拷贝到bin目录的

------

## golang关键字

main

Package main 作为整个程序的入口文件， 在入口文件中必须有main函数进行执行，

import "" 导入语句 

golang函数外只能做标识符变量声明，操作语句必须放入函数中 