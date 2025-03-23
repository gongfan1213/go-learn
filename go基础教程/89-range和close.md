# range 和 close

发送者可通过 close 关闭一个信道来表示没有需要发送的值了。接收者可以通过为接收表达式分配第二个参数来测试信道是否被关闭：若没有值可以接收且信道已被关闭，那么在执行完

```js
v, ok := <- ch
```
这个时候ok会被设置为false
循环for i:=range c会不断从信道当中接受值，直到它被关闭

注意：之应该由发送者关闭信道，不应该由接收者关闭，向一个已经关闭的信道发送数据会引发程序panic

还要注意，信道和文件不同，通常情况下无需关闭他们，只有在必须告诉接收者不再由需要发送的值的时候才有必要关闭，例如终止一个range循环

```js
package main

import (
	"fmt"
)

func fibonacci(n int, c chan int) {
	x, y := 0, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
	}
	close(c)
}

func main() {
	c := make(chan int, 10)
	go fibonacci(cap(c), c)
	for i := range c {
		fmt.Println(i)
	}
}
```
