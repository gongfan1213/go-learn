# for 循环（续）

初始化语句和后置语句是可选的。

https://tour.go-zh.org/flowcontrol/2

```js
package main

import "fmt"

func main() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}
```
