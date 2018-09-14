# 1.3.1 字符串编码

用 Python 写过爬虫的人都应该感受过被字符串编码支配的恐惧。简单来说，编码指的是将可读的字符串转换成不太可读的数字，用来存储或者传输。解码则指的是将数字还原成字符串的过程。常见的编码有 ASCII、GBK 等。

ASCII 编码是一个相当小的字符集合，只有一百多个常用的字符，因此只用一个字节（8 位）就能表示，为了存储本国语言，各个国家都开发出了自己的编码，比如中文的 GBK。这就带来了一个问题，如果我想要在一篇文章中同时写中文和日文，就无法实现了，除非能对每个字符指定编码，这个成本高到无法接受。

Unicode 则是一个最全的编码方式，每个 Unicode 字符占据 6 个字节，可以表示出 2 ^ 48 种字符。但随之而来的是 Unicode 编码后的内容不适合存储和发送，因此诞生了基于 Unicode 的再次编码，目的是为了更高效的存储。

更详细的概念分析和配图说明可以参考这篇文章：[字符串编码入门科普](http://fullstack.blog/2017/09/19/%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%BC%96%E7%A0%81%E5%85%A5%E9%97%A8%E7%A7%91%E6%99%AE/)，这里我们主要聊聊 Python 对字符串编码的处理。

首先，编码的函数是 `encode`，它是字符串的方法：

```python
s = 'hello'
s.encode()         # 得到 b'hello'
s.encode('utf16')  # 得到 b'\xff\xfeh\x00e\x00l\x00l\x00o\x00'
```

`encode` 函数有两个参数，第一个参数不写表示使用默认的 `utf8` 编码，理论上会输出二进制格式的编码结果，但在终端打印时，被自动还原回字符串了。如果用 utf16 进行编码，则会看到编码以后的二进制结果。

前面说过，编码是字符转到二进制的转化过程，有时候在某个编码规范中，并没有指定某个字符是如何编码的，也就是找不到对应的数字，这时候编码就会报错：

```python
city = 'São Paulo'
b_city = city.encode('cp437')
# UnicodeEncodeError: 'charmap' codec can't encode character '\xe3' in position 1: character maps to <undefined>
```

此时需要用到 `encode` 函数的第二个参数，用来指定遇到错误时的行为。它的值可以是 `'ignore'`，表示忽略这个不能编码的字符，也可以是 `'replace'`，表示用默认字符代替：

```python
b_city = city.encode('cp437', errors='ignore') 
# b'So Paulo'
b_city = city.encode('cp437', errors='replace')
# b'S?o Paulo'
```

`decode` 完全是 `encode` 的逆操作，只有二进制类型才有这个函数。它的两个参数含义和 `encode` 函数完全一致，就不再介绍了。

从理论上来说，仅从编码后的内容上来看，是无法确定编码方式的，也无法解码出原来的字符。但不同的编码有各自的特点，虽然无法完全倒推，但可以从概率上来猜测，如果发现某个二进制内容，有 99% 的可能性是 `utf8` 编码生成的，我们就可以用 `utf8 `进行解码。Python 提供了一个强大的工具包 `Chardet` 来完成这一任务：

```python
octets = b'Montr\xe9al'
chardet.detect(octets)
# {'encoding': 'ISO-8859-1', 'confidence': 0.73, 'language': ''}
octets.decode('ISO-8859-1')
# Montréal
```

返回结果中包含了猜测的编码方式，以及可信度。可信度越高，说明是这种编码方式的可能性越大。

有时候，我们拿到的是二进制的字符串字面量，比如 `68 65 6c 6c 6f`，前文说过只有二进制类型才有 `decode` 函数，所以需要通过二进制的字面量生成二进制变量：

```python
s = '68 65 6c 6c 6f'
b = bytearray.fromhex(s)
b.decode()  # hello
```
