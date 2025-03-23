- https://tour.go-zh.org/flowcontrol/3
# for是go当中的[while]

此时你可以去掉分号，因为c当中的while在go中叫做for

```js
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}

```
