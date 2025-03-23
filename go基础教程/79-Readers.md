https://tour.go-zh.org/methods/21

# Readers

IO包指定了io.Reader接口，它表示数据流的读取端，

go标准库包含了该接口的许多的实现，包括文件，网络连接和压缩还有加密等等

io.Reader接口有一个Read方法

```js
func (T) Read(b[] byte)(n int,err error)
```
Read用数据填充给定的字节的切片并且返回填充的字节数和错误值，在遇到数据流的结尾的时候他会返回一个io.EOF的错误

示例代码创建了一个 strings.Reader 并以每次 8 字节的速度读取它的输出。

```js
package main

import (
	"fmt"
	"io"
	"strings"
)

func main() {
	r := strings.NewReader("Hello, Reader!")

	b := make([]byte, 8)
	for {
		n, err := r.Read(b)
		fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
		fmt.Printf("b[:n] = %q\n", b[:n])
		if err == io.EOF {
			break
		}
	}
}
```
