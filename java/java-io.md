# Java IO

标签（空格分隔）： Java

---

## 简介
作用：读写设备上的数据，硬盘文件、内存、键盘、网络...

根据数据的走向，可分为：输入流、输出流
根据处理的数据类型，可分为：字节流、字符流

*字节流*可以处理所有类型的数据，如MP3、图片、文字、视频等。在读取时，读到一个字节就返回一个字节。
*字符流*仅能够处理纯文本数据，如txt文本等。在读取时，读到一个或多个字节，先查找指定的编码表，然后将查找到的字符返。


## 字符、字节与编码
字节（Byte）是通过网络传输信息或在硬盘或内存中存储信息的单位，是计算机信息技术中用于计量存储容量和传输容量的一种计量单位。
1个字节等于8位二进制，即一个8位的二进制数，是一个具体的存储空间，如0x01，0x45等。

字符（Char）是人们使用的记号，抽象意义上的一个符号。
如“1”、“中”、“$”等。

字符集（Charset）也称作编码。各个国家和地区有不同的ANSI编码标准，其内容包含两层含义：
1. 使用哪些字符。也就是说哪些字符会被收入标准中。所包含的“字符”的集合就是字符集；
2. 规定每个字符分别用一个字节还是多个字节存储，用哪些字节来存储，这个规范就叫做编码。

各个国家和地区在指定编码标准的时候，“字符的集合”和“编码”一般是同时制定的。

## 使用字节流读写数据
```java
public class CopyByByteStream {
    public static void main(String[] args) {
        try {
            FileInputStream fis = new FileInputStream("IMG_0205.JPG");
            FileOutputStream fos = new FileOutputStream("IMG_0205_new.JPG");

            byte[] input = new byte[50];
            while (fis.read(input) != -1) {
                fos.write(input);
            }

            fis.close();
            fos.close();

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
缓冲流缓冲区大小指定


## 使用字符流读写数据
```java
public class RWByCharStream {
    public static void main(String[] args) {
        try {
            File file = new File("java.txt");
            FileInputStream fis = new FileInputStream(file);
            FileOutputStream fos = new FileOutputStream("java_new.txt");
            OutputStreamWriter osw = new OutputStreamWriter(fos, "UTF-8");
            InputStreamReader isr = new InputStreamReader(fis, "UTF-8");

            char[] input = new char[100];
            int l = 0;

            while((l = isr.read(input)) != -1) {
                System.out.println(new String(input, 0, l));
                osw.write(input, 0, l);
            }

            isr.close();
            fis.close();
            osw.close();
            fos.close();

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
