# Linux系统中万物皆文件

## Linux命令一般的格式

 `命令名 [选项] 参数1 参数2 ....` 

- 选项可以组合使用



# 文件管理

## 目录结构 <span id="yigemaodian"> </span> 

> 以Windows下的C盘为例

```
C:
|---- Intel
|---- Program Files
|---- Program Files (x86)
|---- Windows
|---- Users
```



## 路径

> 绝对路径以 `/` 开头，相对路径不以 `/` 开头

 `./ 或 .` ：当前文件夹

 `..` ：上一级文件夹

 `/` ：根目录

 `~` ：home目录

### 父/子目录

如果存在一下目录结构

```
Pictures
|---- Wallpaper
|---- Saved Pictures
```

那么 `Pictures` 就是 `Wallpaper` 和 `Saved Pictures` 的父目录， `Wallpaper` 和 `Saved Pictures` 就是 `Pictures` 的子目录



## 展示目录结构 `tree` 

 `tree` ：输出效果跟 [上面](#yigemaodian) 类似



## 展示目录下的内容 `ls` 

> list directory contents，默认按首字母排序

 `ls -a` ：显示当前文件夹下的所有文件和子目录，包括隐藏的文件（一般以 `.` 开头的文件就是隐藏文件）

 `ls -R` ：显示所有的文件（包括子目录下的文件），同时展开所有的子目录

 `ls -l` ：除了文件名外，还包括了文件的权限、创建者、大小、创建日期和名字等

--------------------------

#####  `ls -l` 输出的含义 

 `drwxr-xr-x. 2 root root 18 Nov 14 19:49 a` 

- 第一部分 `drwxr-xr-x` 

  - 第1位：表示文件类型。 `-` 表示普通文件， `d` 表示文件夹， `l` 表示连接文件。特殊文件： `b` ， `p` ， `c` ， `s` ....

  - 第2~4位：依次表示属主（文件创建者）对这个文件或文件夹拥有的权限， `r` 表示读（read）权限，数字表达为 `4` ； `d` 表示写（write）权限，数字表达为 `2` ； `x` 表示运行（execute）权限，数字表达为 `1` 
  - 第5~7位：依次表示属主所在用户组的其他用户对这个文件的权限
  - 第8~10位：依次表示其他用户组的用户对这个文件的权限

- 第二部分 `2` 
  - 文件硬链接数或目录的子目录数

- 第三部分 `root` 
  - 属主（文件创建者）

- 第四部分 `root` 
  - 属组（创建文件的用户所在用户组）

- 第五部分 `18` 
  - 文件的大小，单位字节（byte）

- 第六部分 `Nov 14 19:49` 
  - 文件最后修改时间

- 第七部分 `a` 
  - 文件全名

------------------------------------------------------

 `ls -t` ：按文件修改时间的先后进行排序，日期越新，位置越靠前

 `ls -s` ：按文件大小进行排序

-  `total` ：数据块（block）概念，单位是千字节（kb）

 `ls -r` ：以相反的顺序排列



## 展示当前文件夹的绝对路径 `pwd` 

 `pwd` ：输出应该是 `/home/cloudcode/Downloads` 



## 创建目录 `mkdir` 

> make directory

在当前目录下创建多个目录

 `mkdir directory_name1 directory_name2` 以空格隔开

 `mkdir {directory_name1,directory_name2}` 用花括号括起来，并用 `,` 隔开

- 例如： `mkdir test1 test2` 和 `mkdir {test1,test2}` 都表示在当前目录下创建 `test01` 和 `test02` 目录



在指定路径（目录）下创建目录

 `mkdir 你指定的路径/directory_name` 



当路径中的目录不存在时，自动创建

 `mkdir -p 路径/文件` 

- 例如：创建 `test03/a` 目录，执行 `mkdir test03/a` ，会报错提示 `test03` 目录不存在。加上 `-p` 后就不会提示了



## 创建文件 `touch` 

 `touch file_name` ：创建一个类型为空的空文件

- 例如：先执行 `touch test04` 创建一个名为 `test04` 的文件，在执行 `file test04` ，就会发现 `test04` 的类型是 `empty` ，也就是说它的文件类型为空。再执行 `cat test04` ，没有输出任何东西，说明 `test04` 是一个空文件（里面什么都没有）

  - 注意：如果在创建文件时，没有指定文件类型（即拓展名），那么当你在文件内添加内容并保存后，该文件默认的类型是 `.txt` 

    例如：执行 `touch sample` ，再双击将其打开，粘贴以下内容

    ```html
    <!DOCTYPE html>
    <html>
    	<head>
    		<title>Sample</title>
    	</head>
    	<body>
    		<h1>Hello, Linux!</h1>
    	</body>
    </html>
    ```

    保存后发现文件夹中的 `sample` 文件拓展名为 `.txt` ，再次双击该文件打开的依旧是文本编辑的界面

  - 注意：即便在创建文件时指定了文件类型，在没添加相应内容前，该文件的类型依旧是空

    例如：先执行 `touch sample.html` ，再执行 `file sample.html` ，输出的文件类型为 `empty` 。此时我们双击将其打开，粘贴以下内容

    ```html
    <!DOCTYPE html>
    <html>
    	<head>
    		<title>Sample</title>
    	</head>
    	<body>
    		<h1>Hello, Linux!</h1>
    	</body>
    </html>
    ```

    保存后发现文件夹中的 `sample.html` 文件的图标已经变化，执行 `file sample.html` ，输出的文件类型为 `HTML document, ASCII text` ，双击该文件可在浏览器中正常打开。




## 移动文件 / 目录 `mv` 

> move file

 `mv 要移动的文件/目录 目标目录` 



## 重命名文件 / 目录 `mv` 

 `mv 原文件名 新文件名` ：跟名字时，作用就是改名

 `mv 原文件名 路径/新文件名` ：加上路径时，作用就是移动的同时改名



## 复制粘贴文件 / 目录 `cp` 

> copy file，cp不能复制目录，如果要用来复制目录，需要加上 `-R 或 -r` 

 `cp -R 或 cp -r` ：递归地将文件复制到目标目录下，但是最后修改时间等信息不会被复制

 `cp -p` ：除复制文件的内容外，还把修改时间和访问权限也复制到 **新文件** 中（需要有两个普通用户才好演示，记住就好，学地深入之后可自行验证，讲多了就昏了）

 `cp -a` ：和Windows下的复制粘贴一样（首推），效果同 `-dpR` 

 `cp -f` ：覆盖已经存在的目标 **文件** 而不给出提示

 `cp -i` ：在覆盖目标文件之前给出提示，要求用户确认是否覆盖，输入 `y` 并回车（或者直接回车）时目标文件将被覆盖



## 删除文件 `rm` 

> 永久删除，不可逆的

 `rm file_name` ：删除文件

 `rm *` ：删除当前目录下的所有文件

 `rm -i` ：删除文件前逐一询问

 `rm -r directory_name` ：删除名字为 `directory_name` 的目录和该目录内的所有子目录和文件

 `rm -f` ：强制删除文件（不管有没有权限都删了，而且不给出提示）

- 例如：执行 `rm -r aa` 会给出提示（提示内容大概就是你没得 `w` 权限，回车跳过跳过就行），此时执行 `ls` 发现这个目录还在。如果我们加上 `-f` 选项后（ `rm -rf aa` ），就可以直接删除 `aa` 。（还是使用 `ls` 验证）



## 删除目录 `rmdir` 

> remove directory

 `rmdir directory_name` ：删除名字为 `directory_name` 的目录

 `rmdir -p directory_name` ：如果目标目录被删除后，其父目录就变成空目录的话，就把父目录一并删除



## 归档与压缩

> tar文件归档打包工具：将多个文件或目录打包成一个文件

归档就是把零散的文件集中起来，起到整理、备案的作用；压缩能够减小体积、节省磁盘空间

 `-c` ：创建归档文件

 `-f` ：指定要操作的归档文件

 `--exclude` ：排除某个文件

 `-r` ：在归档尾部追加文件

 `-t` ：列出归档文件中有哪些文件

 `-x` ：提取归档文件

 `-C` ：指定提取位置

 `-v` ：显示归档过程

> tar本身不具备压缩功能，可以通过调用其他压缩工具来压缩

 `-z` ：使用gzip压缩

 `-j` ：使用bzip2压缩

 `-J` ：使用xz压缩



先执行以下命令

 `mkdir test_dir aa bb && touch f.txt test_dir/{g.txt,h.txt}` ：创建目录 `test_dir` ，之后再创建 `f.txt` 和 `test_dir/g.txt` 



例如：

-   `tar -cf a.tar test_dir` ：将 `test_dir` 归档到 `a.tar` 中
    - 使用 `file a.tar` 可以看到其类型为 `POSIX tar archive (GNU)` 
    - 使用 `ls -l a.tar` 可以看到其大小为 `10240` 
    - 使用 `tar -tf a.tar` 可以看到归档内的文件有哪些
-   `tar -rf a.tar f.txt` ：将 `f.txt` 追加到 `a.tar` 的尾部
    - 使用 `tar -tf a.tar` 可以验证追加是否成功
-   `tar --exclude=test_dir/g.txt -cf b.tar test_dir` ：除了 `g.txt` 外， `test_dir` 下的所有文件都归档到 `b.tar` 中
    - 使用 `file b.tar` 可以看到其类型为 `POSIX tar archive (GNU)` 
    - 使用 `ls -l b.tar` 可以看到其大小为 `10240` 
    - 使用 `tar -tf b.tar` 可以看到归档内确实没有 `test_dir/g.txt` 
-   `tar -xf a.tar -C aa` ：将 `a.tar` 中的文件提取到 `aa` 目录下
    -  使用 `ls -lR aa` 可以看到提取成功了

-   `tar -czvf a.tar.gz test_dir` ：先使用 `tar` 将 `test_dir` 归档，再使用gzip压缩这个归档，压缩包名字叫 `a.tar.gz` 
    - 使用 `ls -l a.tar.gz` 可以看到其大小为 `158` 
      - 由此可见归档和压缩是不一样的
-   `tar -xzvf a.tar.gz -C bb` ：将 `a.tar.gz` 解压并解包（或者叫解档？）到 `bb` 目录下
    -  使用 `ls -lR bb` 可以看到操作成功了


使用bzip2和xz与上面类似，只需要把选项中的 `z` 换成对应的选项即可



### 第四种压缩 `zip` 

> zip的方式使用不一样

压缩： `zip 压缩包的名字 要压缩的文件1 要压缩的文件2 要压缩的文件3 ....` 

解压： `unzip 压缩包的名字` 



## 查找文件

 ` locate` ：

 `find` ：

 `which` ：



# 文本操作

##  `cat` 

> con**cat**enate的缩写，用于小文本文件的查看。Linux中可以使用 `PgUp` + `↑` 向上翻页，但大文本文件还是不要用 `cat` 来查看

 `-n` ：对输出的所有行进行编号

 `-b` ：只对非空行进行编号

 `-s` ：将文件中连续两行以上的空行以一个空行输出

 `-v` ：使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外

 `-E` ：列出每行结尾的回车符 `$` 

 `-T` ：把 Tab 键用 `^I` 显示出来

 `-A` ：展示文本内的所有内容，包括所有隐藏符号，等价于 `-vET` 

 `>` ：重定向运算符。新建文件或者需要把文件内容加入到指定文件中时，会用到这个符号（如果该文件不存在，则会自动创建，否则这个操作会覆盖指定文本中的内容）

```bash
## 展示目录结构，不存在e1、e2
ls

## 该文件不存在，则会自动创建
cat > e1
这是文件1的内容

cat > e2
这是文件2的内容

## 覆盖指定文本中的内容
cat e1 > e2
```

 `>>` ：将文本内容追加到指定文件的尾部

```bash
## 创建文件e3
cat > e3
e3中的内容会被追加到文件尾部，而不是覆盖

## 同样地，该文件不存在，则会自动创建
cat e3 >> e2
```







##  `more` 

> 相较于 `cat` ， `more` 会一页一页地显示文件内容，更适合阅读长文本。使用 `more` 时会打开一个交互界面

### 交互界面命令

 `空格` ：往下翻一页

 `回车` ：往下翻一行

 `=` ：输出当前的行号

 `b` ：往上翻一页

 `/pattern` ：在下一页搜索指定字符串，然后从该字符串的前两行开始显示

 `q` 或 `Q` ：退出



### 选项

 `-n` ：一次显示的行数（自动换行后的行数，而不是实际行数）

 `+n` ：从第 `n` 行开始显示

 `-s` ：当遇到有连续两行以上的空白行，就代换为一行的空白行

 `-p` ：**不以卷动**的方式显示每一页，而是先清除萤幕后再显示内容

 `-c` ：跟 `-p` 相似，不同的是先显示内容再清除其他旧资料

 `-f` ：计算行数时，按实际行数计算，而非自动换行过后的行数（单行字数太长时会自动换行）

 `+/pattern` ：在每个文档显示前搜寻该字符串（pattern），然后从该字符串的前两行开始显示



##  `less` 

> 

 



##  `head` 

 `-n num` ：显示开头的 `num` 行，默认是 `-n 10` ，即显示开头的10行

 `-c num` ：显示文件前 `num` 个字节的内容



##  `tail` 





##  `grep` 

> 在文件中查找符合条件的字符串

 `grep [选项] 目标字符串 目标文件` 

 `-c` ：计算文件中有多少行包含目标字符串

 `-i` ：在查找时忽略大小写

 `-o` ：只显示目标字符串



##  `awk` 





## vim

### 设置

> 修改 `.vimrc` 文件进行设置，执行 `vim --version` 找到 `.vimrc` 文件的位置

#### 显示行号

```
set number
```

此时，在vim编辑器中输入 `nj` ，光标会向下移动 `n` 行



#### 显示相对行号

```
set relativenumber
```



### 命令模式 `Normal Model` 

> 进入vim时就处于命令模式，此时输入的字符会被当作命令

 `k` ：向上移动光标，等同于按下 Up 键

 `j` ：向下移动光标，等同于按下 Down 键

 `h` ：向左移动光标，等同于按下 Left 键

 `l` ：向右移动光标，等同于按下 Right 键

 `i` （insert）：插入到光标左侧，并自动进入编辑模式

 `a` （append）：插入到光标右侧，并自动进入编辑模式

 `Shift` + `i` 或 `I` ：插到当前行的最前面，并自动进入编辑模式

 `Shift` + `a` 或 `A` ：插到当前行的最后面，并自动进入编辑模式

 `o` （open new line）：向下新增一行，并自动进入编辑模式

 `O` 或 `shift + o` ：向上新增一行，并自动进入编辑模式

 `G` ：将光标移动到最后一行，在前面加上数字跳转到对应的行

 `line_number` + `G` ：移动到 `line_number` 行，如 34G 即移动到第34行

 `gg` ：将光标移动到文件第一行，在前面加上数字跳转到对应的行

 `yy` ：复制当前行

 `number` + `yy` ：从光标所在行开始，向下复制 number 行

 `yw` ：复制光标所在的单词

 `p` （paste）：粘贴在光标所在行的下一行（在前面加上数字 `n` 表示粘贴 `n` 次）

 `P` ：粘贴在光标所在行的上一行

 `d` ：删除

 `dd` ：删除当前行

 `.` ：重复上次的操作

 `u` （undo）：撤销

 `Ctrl` + `r` ：撤销上一次的撤销操作

 `dw` ：删除单词（将光标移动到单词首字母的左边）

 `cw` ：改变单词（将光标移动到单词首字母的左边），自动进入编辑模式

 `w` ：移动到下一个单词的首字母左边（数字、符号都会被视作一个单词）

 `b` ：移动到下一个单词的首字母左边（数字、符号都会被视作一个单词）

 `/pattern` ：搜索指定的字符串

 `Ctrl` + `v` ：可视化块，从光标处开始选中

 `Shift` + `v` ：可视化行，从光标所在行开始选中





### 插入模式 `Insert Mode` 

> 插入模式下。按下 `esc` 即可进入命令模式

- **字符按键和Shift的组合**：输入字符
- **ENTER**：回车键，换行
- **BACK SPACE**：退格键，删除光标前一个字符
- **DEL**：删除键，删除光标后一个字符
- **方向键**，在文本中移动光标
- **HOME**/**END**，移动光标到行首/行尾
- **Page Up**/**Page Down**，上/下翻页
- **Insert**，切换光标为输入/替换模式，光标将变成竖线/下划线
- **ESC**，退出输入模式，切换到命令模式

绝对行号：相对第 `1` 行而言，第 `n` 行的行号为第 `n-1` 行

相对行号：相对光标所在行（如第 `i` 行）而言，第 `n` 行的相对行号为 `n-i` 行



### 底线命令模式 `Last Line Mode` 

> 在命令模式输入 `:` 时，进入底线命令模式

 `:q` ：退出。在没有进行修改时使用，否则会报错

 `:q!` ：不保存并强制退出，在不希望保存所做修改时使用

 `:w` ：保存文件但不退出

 `:wq` 或者 `:x` ：保存并退出

 `:wq!` ：强制保存并退出

 `:%s/旧字符/新字符/g` ：将旧字符替换为新字符。g表示global，也就是全局替换

 `:%s/旧字符/新字符/gc` ：g表示global，c表示confirm，与上一条的区别在于会在替换给出提示让用户确认是否替换（每个词都需要确认）