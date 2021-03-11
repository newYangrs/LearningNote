# Linux中grep命令详解

## 1.作用

   Linux系统中grep命令是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹 配的行打印出来。grep全称是Global Regular Expression Print，表示全局正则表达式版本，它的使用权限是所有用户。
   grep家族包括grep、egrep和fgrep。egrep和fgrep的命令只跟grep有很小不同。egrep是grep的扩展，支持更多的re元字符， fgrep就是fixed grep或fast grep，它们把所有的字母都看作单词，也就是说，正则表达式中的元字符表示回其自身的字面意义，不再特殊。linux使用GNU版本的grep。它功能更强，可以通过-G、-E、-F命令行选项来使用egrep和fgrep的功能。

##    2.格式及主要参数

   grep [options]
   主要参数：  grep --help可查看
     -c：只输出匹配行的计数。
     -i：不区分大小写。
     -h：查询多文件时不显示文件名。
     -l：查询多文件时只输出包含匹配字符的文件名。
     -n：显示匹配行及 行号。
     -s：不显示不存在或无匹配文本的错误信息。
     -v：显示不包含匹配文本的所有行。
     --color=auto ：可以将找到的关键词部分加上颜色的显示。
   pattern正则表达式主要参数:
   \： 忽略正则表达式中特殊字符的原有含义。
   ^：匹配正则表达式的开始行。
   $: 匹配正则表达式的结束行。
   \<：从匹配正则表达 式的行开始。
   \>：到匹配正则表达式的行结束。
   [ ]：单个字符，如[A]即A符合要求 。
   [ - ]：范围，如[A-Z]，即A、B、C一直到Z都符合要求 。
   .：所有的单个字符。
   *：所有字符，长度可以为0。

##    3.grep命令使用简单实例

   itcast$ grep ‘test’ d*
   显示所有以d开头的文件中包含 test的行

   itcast $ grep ‘test’ aa bb cc
   显示在aa，bb，cc文件中匹配test的行。

   itcast $ grep ‘[a-z]\{5\}’ aa
   显示所有包含每个字符串至少有5个连续小写字符的字符串的行。

   itcast $ grep ‘wesest.*\1′ aa
     如果west被匹配，则es就被存储到内存中，并标记为1，然后搜索任意个字符(.*)，这些字符后面紧跟着 另外一个es(\1)，找到就显示该行。如果用egrep或grep -E，就不用”\”号进行转义，直接写成’w(es)t.*\1′就可以了。

##    4.grep命令使用复杂实例

   明确要求搜索子目录：
   grep -r
   或忽略子目录
   grep -d skip
   如果有很多输出时，您可以通过管道将其转到’less’上阅读：
   itcast$ grep magic /usr/src/Linux/Documentation/* | less
   这样，您就可以更方便地阅读。
   有一点要注意，您必需提供一个文件过滤方式(搜索全部文件的话用 *)。如果您忘了，’grep’会一直等着，直到该程序被中断。如果您遇到了这样的情况，按 ，然后再试。
   下面还有一些有意思的命令行参数：
   grep -i pattern files ：不区分大小写地搜索。默认情况区分大小写，
   grep -l pattern files ：只列出匹配的文件名，
   grep -L pattern files ：列出不匹配的文件名，
   grep -w pattern files ：只匹配整个单词，而不是字符串的一部分(如匹配’magic’，而不是’magical’)，
   grep -C number pattern files ：匹配的上下文分别显示[number]行，
   grep pattern1 | pattern2 files ：显示匹配 pattern1 或 pattern2 的行，
   例如：grep "abc\|xyz" testfile 表示过滤包含abc或xyz的行
   grep pattern1 files | grep pattern2 ：显示既匹配 pattern1 又匹配 pattern2 的行。
   grep -n pattern files 即可显示行号信息
   grep -c pattern files 即可查找总行数
   还有些用于搜索的特殊符号：\< 和 \> 分别标注单词的开始与结尾。
   例如：
   grep man * 会匹配 ‘Batman’、’manic’、’man’等，
   grep ‘\<man’ * 匹配’manic’和’man’，但不是’Batman’，
   grep ‘\<man\>’ 只匹配’man’，而不是’Batman’或’manic’等其他的字符串。
   ‘^’： 指匹配的字符串在行首，
   ‘$’： 指匹配的字符串在行 尾，

   用grep查找结构体 grep -R "struct task_struct {" /usr/src 加-n可以显示行号
PS1=$ 进入到家目录在.bashrc   中



from：https://blog.csdn.net/qq_40797605/article/details/89075918?ops_request_misc=%25257B%252522request%25255Fid%252522%25253A%252522161337289016780274127413%252522%25252C%252522scm%252522%25253A%25252220140713.130102334..%252522%25257D&request_id=161337289016780274127413&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-89075918.pc_search_result_no_baidu_js&utm_term=grep