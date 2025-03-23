https://tour.go-zh.org/moretypes/12

# nil切片

切片的零值是nil

nil的切片长度和容量都是0并且没有底层数组

```js
package main

import "fmt"

func main() {
	var s []int
	fmt.Println(s, len(s), cap(s))
	if s == nil {
		fmt.Println("nil!")
	}
}

```
