## excel 转lua配置

xlsx 文件格式如下图：  
![模板](xlsx.jpg)  

**特性：**

- xlsx生成lua文件
- 支持id重复检测
- 支持json格式检测  
~~- 支持md5值检测文件是否有变动（因为会拖累生成速度，干脆暴力，每次全都生成一次）~~

**配置头部定义：**  

- 第一行为字段注释
- 第二行为字段名，英文开头，不允许带空格，不允许出现中文和特殊符号
- 第三行为字段类型，包括 int，number，table，string，注意：table为json字符串，例如，[["hit",8,10],["cri",8,10]] ，表示一个json数组
- 第四行为生成模式，c(client)表示客户端专用，s(server)表示服务器专用，d(double)表示双端都要用，r(remark)表示双端都不需要生成


**参数：**

- -i xxx 表示xlsx文件路径
- -o yyy 表示生成lua文件的路径
- -l aaa 表示翻译文件路径 （为了本地化，不填则不翻译）

> 翻译的原理是：在生成lua的同时，检查字段是否是string并且模式是s或者d，然后检查该字段是否有相应的翻译字符串对应，如果有则替换。

例子：

***windows：***  
`xlsx2lua.exe -i xlsx -o l-xlsx -l language\en`

表示 将`xlsx/`文件夹下的所有xlsx装换成lua，lua文件的输出路径为`/l-xlsx`,并且在转换的同时翻译相关字段，翻译文件的路径为：`language\en`


***linux:***  
`xlsx2lua.exe -i xlsx -o l-xlsx -l language/en`

注意：linux与windows区别在，路径斜杠和反斜杠的问题，请注意。

## 压缩
go生成的可执行文件有点大，可以采用下面两个步骤减小二进制文件大小。

1. 在build时，加入参数`go build -ldflags "-w -s"` 
2. 使用upx加壳压缩

**log**
- 2017-10-23

	1. 增加新的字段生成方式r（remark）,表示这个字段服务器和客户端都不需要生成，只是用来做备注，给策划或开发看的。