# 编码相关

### UTF-8编码原则

UTF-8是一种变长字节编码方式。对于某一个字符的UTF-8编码，如果只有一个字节则其最高二进制位为0；**<u>*如果是多字节，其第一个字节从最高位开始，连续的二进制位值为1的个数决定了其编码的位数，其余各字节均以10开头。*</u>**UTF-8最多可用到6个字节。 

```
如表： 

1字节 0xxxxxxx 
2字节 110xxxxx 10xxxxxx 
3字节 1110xxxx 10xxxxxx 10xxxxxx 
4字节 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx 
5字节 111110xx 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx 
6字节 1111110x 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx 
```

因此UTF-8中可以用来表示字符编码的实际位数最多有<u>31位</u>，即上表中x所表示的位。除去那些控制位（每字节开头的10等），这些x表示的位与UNICODE编码是一一对应的，位高低顺序也相同。 

实际将UNICODE转换为UTF-8编码时应先去除高位0，然后根据所剩编码的位数决定所需最小的UTF-8编码位数。 
因此那些基本ASCII字符集中的字符（UNICODE兼容ASCII）只需要一个字节的UTF-8编码（7个二进制位）便可以表示。 

对于上面的问题，代码中给出的两个字节是 
十六进制：C0 B1 
二进制：11000000 10110001 
对比两个字节编码的表示方式： 
110xxxxx 10xxxxxx 
提取出对应的UNICODE编码： 
00000 110001 
可以看出此编码并非“标准”的UTF-8编码，因为其第一个字节的“有效编码”全为0，去除高位0后的编码仅有6位。由前面所述，此字符仅用一个字节的UTF-8编码表示就够了。 
<u>JAVA在把字符还原为UTF-8编码时，是按照“标准”的方式处理的，因此我们得到的是仅有1个字节的编码。</u> 



### 字节流为什么会出现乱码

**因为字字节流一次读一个字节，而不管是GBK还是UTF-8一个中文都是多个字节，用字节流只能读取期中的一部分，所以会出现乱码的问题。**

#### Ex1. 没有出现乱码



```java
public static void main(String[] args){
  FileOutputStream fos = new FileOutputStream(a.txt);
  fos.write("这不会乱码".getBytes());
}
```

#### Ex2. 出现乱码

```java
public static void main(String[] args) throws IOException {
    FileInputStream fis = new FileInputStream("javase/day11/a.txt");
    int b = 0;

    while( (b = fis.read()) != -1){
       System.out.print((char)b);
    }
  	fis.close();
}
/*
a.txt
03, 方楚, 21
04, 贾伟, 21
05, 周智贤, 21

sout:
03, æ¹æ¥, 21
04, è´¾ä¼, 21
05, å¨æºè´¤, 21
*/
   
```

*在这种情况下，如果使用*

```java
byte[] b = new byte[1024];
int read = 0;
while ((read = fis.read(b)) != -1){
  System.out.print(new String(b, 0, read));
}
```

*则不会有乱码的问题，因为一次读取了1024个字节[但可能字节太多还是会有问题？？]*

![image-20200807144050095](img/image-20200807144050095.png)

```java
byte[] b = new byte[1024];
int read = 0;
while ( fis.read(b) != -1 ){
  System.out.print(new String(b, "utf-8"));
}
```

