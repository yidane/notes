---
layout: default
title: 字符串
nav_order: 4
parent: Go
grand_parent: 编程语言
---

# golang字符串处理

几乎任何程序都离不开字符串，字符串是 UTF-8 字符的一个序列（当字符为 ASCII 码时则占用 1 个字节，其它字符根据需要占用 2-4 个字节）。

Go语言字符串是一种值类型，且值不可变，即创建某个文本后你无法再次修改这个文本的内容；更深入地讲，字符串是字节的定长数组。

Go 代码使用 UTF-8 编码（且不能带 BOM），同时标识符支持 Unicode 字符。在标准库 unicode 包及其子包 utf8、utf16中，提供了对 Unicode 相关编码、解码的支持，同时提供了测试 Unicode 码点（Unicode code points）属性的功能。

Go语言中的字符串也可能根据需要占用 1至 4 个字节，这与其它语言如 C++、Java 或者 Python 不同（Java 始终使用 2 个字节）。Go 这样做的好处是不仅减少了内存和硬盘空间占用，同时也不用像其它语言那样需要对使用 UTF-8 字符集的文本进行编码和解码。

Go语言支持以下2种形式的字符串：

首先看下以下例子：

package main

import "fmt"

func main(){

    s := "我是中国人"

 

    for i:=0; i < len(s); i++{

         fmt.Printf("%c", s[i])

    }

 

    fmt.Printf("\n")

 

    for _, v := range s {

        fmt.Printf("%c", v)

    }

 

    fmt.Print("\n")

}

输出结果：

ææ¯ä¸­å½äºº

我是中国人



通过len(s)和range遍历访问字符串元素有什么不同么？



首先来复习一下go语言的字符串表示，go语言有以下两种表示字符串的方法：

1、双引号，如：“gogogo\n”，使用转义字符

2、反引号，如：`gogogo\n`，不使用转义字符，字符串的内容将和赋值保持严格一致



在go语言中，没有字符类型，rune就是字符类型



Go 语言中的字符串是以 UTF-8 格式编码并存储的，例如：



s := "Hello 世界！"



　　变量 s 中存放的是这个字符串的 UTF-8 编码，当你使用 len(s) 函数获取字符串的长度时，获取的是该字符串的 UTF-8 编码长度。通常我们认为，在电脑中存储一个 ASCII 字符需要一个字节，存储一个非 ASCII 字符需要两个字节，这种认为仅仅是针对 Windows 系统中常用的 ANSI 编码而言的，而在 Go 语言中，使用的是 UTF-8 编码，用 UTF-8 编码来存放一个 ASCII 字符依然只需要一个字节，而存放一个非 ASCII 字符，则需要 2个、3个、4个字节，它是不固定的。



　　既然 Go 中的字符串存放的是 UTF-8 编码，那么我们使用 s[0] 这样的下标方式获取到的内容就是 UTF-8 编码中的一个字节。对于非 ASCII 字符而言，这样的一个字节没有实际的意义，除非你想编码或解码 UTF-8 字节流。而在 Go 语言中，已经有很多现成的方法来编码或解码 UTF-8 字节流了。



遍历字符串中的字节（使用下标访问）：



func main() {

 s := "Hello 世界！"

 for i, l := 0, len(s); i < l; i++ {

 fmt.Printf("%2v = %v\n", i, s[i]) // 输出单个字节值

 }

}



遍历字符串中的字符（使用 for range 语句访问）：



func main() {

 s := "Hello 世界！"

 for i, v := range s { // i 是字符的字节位置，v 是字符的拷贝

 fmt.Printf("%2v = %c\n", i, v) // 输出单个字符

 }

}

　　在 Go 语言中，字符串的内容是不能修改的，也就是说，你不能用 s[0] 这种方式修改字符串中的 UTF-8 编码，如果你一定要修改，那么你可以将字符串的内容复制到一个可写的缓冲区中，然后再进行修改。这样的缓冲区一般是 []byte 或 []rune。如果要对字符串中的字节进行修改，则转换为 []byte 格式，如果要对字符串中的字符进行修改，则转换为 []rune 格式，转换过程会自动复制数据。

修改字符串中的字节（用 []byte）：

func main() {

 s := "Hello 世界！"

 b := []byte(s)        // 转换为 []byte，自动复制数据

 b[5] = ','            // 修改 []byte

 fmt.Printf("%s\n", s) // s 不能被修改，内容保持不变

 fmt.Printf("%s\n", b) // 修改后的数据

}

修改字符串中的字符（用 []rune）：

func main() {

 s := "Hello 世界！"

 r := []rune(s)         // 转换为 []rune，自动复制数据

 r[6] = '中'            // 修改 []rune

 r[7] = '国'            // 修改 []rune

 fmt.Println(s)         // s 不能被修改，内容保持不变

 fmt.Println(string(r)) // 转换为字符串，又一次复制数据

}



　　在 []byte 中处理 Rune 字符（需要用到 utf8 包中的解码函数）



func main() {

 b := []byte("Hello 世界！")

 for len(b) > 0 {

 r, n := utf8.DecodeRune(b) // 解码 b 中的第一个字符

 fmt.Printf("%c\n", r)      // 显示读出的字符

 b = b[n:]                  // 丢弃已读取的字符

 }

}

