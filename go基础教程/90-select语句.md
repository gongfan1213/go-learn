# select语句

select语句可以让一个go程等待多个通信操作

select会阻塞到某个分支可以继续执行为止，这个时候就会执行该分支，当多个分支都准备好的时候会随机选择一个执行

```js
package main

import "fmt"

func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
		case c <- x:
			x, y = y, x+y
		case <-quit:
			fmt.Println("quit")
			return
		}
	}
}

func main() {
	c := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	fibonacci(c, quit)
}

```
