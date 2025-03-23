# 练习：Reader
实现一个 Reader 类型，它产生一个 ASCII 字符 'A' 的无限流。

```js
package main

import "golang.org/x/tour/reader"

type MyReader struct{}

// TODO: 为 MyReader 添加一个 Read([]byte) (int, error) 方法。

func main() {
	reader.Validate(MyReader{})
}
```


```js
type MyReader struct{}

// 实现 Read 方法，用 'A' 填充字节数组
func (r MyReader) Read(p []byte) (n int, err error) {
    for i := range p {
        p[i] = 'A'
    }
    return len(p), nil
}

func main() {
    reader.Validate(MyReader{})
}
```
