#  Linux压缩



### .gz：由gzip压缩工具压缩的文件。

- 只能打包单个文件，不能打包文件夹
- 压缩
  - gzip *
    - txtfile1.txt txtfile2.txt txtfile3.txt -> txtfile1.gzip txtfile2.gzip txtfile3.gzip
- 解压
  - gunzip txtfile1.gzip
  - 将文件解压，压缩包删除

### .bz2：由bzip2压缩工具压缩的文件。

".bz2"格式是 Linux 的另一种压缩格式，从理论上来讲，".bz2"格式的算法更先进、压缩比更好；而 咱们上面学到的".gz"格式相对来讲时间更快。若没有加上任何参数，bzip2压缩完文件后会产生.bz2的压缩文件，并删除原始的文件。

- 压缩

  - bzip2 txtfile1.txt
  - txtfile1.txt -> txtfile1.txt.bz2

- 解压

  - bunzip2 txtfile.txt.bz2

  

### .tar：由tar打包程序打包的文件（tar没有压缩功能，只是把一个 目录合并成一个文件）

```shell
First option must be a mode specifier:
  -c Create  -r Add/Replace  -t List  -u Update  -x Extract
```

- 将txtfile.txt打包
  - tar -zcvf txt.tar.gz txtfile.txt 

- 先用tar进行打包（没压缩
  - tar -cvf * txt.tar
- 再用gzip进行压缩
  - gzip txt.tar

```shell
-c create
-v be verbose
-f <filename> Location of archive
```

- 查看tar中有哪些文件

  - tar -ztvf txt.tar.gz 

- 解压缩

  - tar -zxvf txt.tar.gz

  - 如果当前目录存在文件，则覆盖（？）

  - 不存在则生成，且不会删除压缩包

    

### .zip:可理解为由zip压缩工具直接压缩

```shell
-r   recurse into directories  
```

- 压缩

  - zip -r zFi.zip *

- 解压

  - unzip -d ./unFiles zFiles.zip

  ```shell
  -d  extract files into exdir
  ```

  