# 练习：Web 爬虫
在这个练习中，我们将会使用 Go 的并发特性来并行化一个 Web 爬虫。

修改 Crawl 函数来并行地抓取 URL，并且保证不重复。

*提示：* 你可以用一个 map 来缓存已经获取的 URL，但是要注意 map 本身并不是并发安全的！

```go
package main

import (
	"fmt"
	"sync"
)

type Fetcher interface {
	// Fetch 返回 URL 所指向页面的 body 内容，
	// 并将该页面上找到的所有 URL 放到一个切片中。
	Fetch(url string) (body string, urls []string, err error)
}

// Crawl 用 fetcher 从某个 URL 开始递归的爬取页面，直到达到最大深度。
func Crawl(url string, depth int, fetcher Fetcher) {
	// 使用一个 map 来缓存已经获取的 URL，保证不重复爬取
	visited := make(map[string]bool)
	// 使用互斥锁来保证对 visited map 的并发安全访问
	var mu sync.Mutex

	// 创建一个带缓冲的信道来接收结果
	resultChan := make(chan string, 100)
	// 创建一个 WaitGroup 来等待所有协程完成
	var wg sync.WaitGroup

	// 定义一个内部函数来处理每个 URL 的爬取
	var crawl func(string, int)
	crawl = func(u string, d int) {
		defer wg.Done()

		mu.Lock()
		if visited[u] {
			mu.Unlock()
			return
		}
		visited[u] = true
		mu.Unlock()

		if d <= 0 {
			return
		}

		body, urls, err := fetcher.Fetch(u)
		if err != nil {
			fmt.Println(err)
			return
		}
		resultChan <- fmt.Sprintf("found: %s %q\n", u, body)

		for _, newUrl := range urls {
			wg.Add(1)
			go crawl(newUrl, d-1)
		}
	}

	wg.Add(1)
	go crawl(url, depth)

	// 等待所有协程完成
	go func() {
		wg.Wait()
		close(resultChan)
	}()

	// 从信道中读取并打印结果
	for res := range resultChan {
		fmt.Print(res)
	}
}

func main() {
	Crawl("https://golang.org/", 4, fetcher)
}

// fakeFetcher 是待填充结果的 Fetcher。
type fakeFetcher map[string]*fakeResult

type fakeResult struct {
	body string
	urls []string
}

func (f fakeFetcher) Fetch(url string) (string, []string, error) {
	if res, ok := f[url]; ok {
		return res.body, res.urls, nil
	}
	return "", nil, fmt.Errorf("not found: %s", url)
}

// fetcher 是填充后的 fakeFetcher。
var fetcher = fakeFetcher{
	"https://golang.org/": &fakeResult{
		"The Go Programming Language",
		[]string{
			"https://golang.org/pkg/",
			"https://golang.org/cmd/",
		},
	},
	"https://golang.org/pkg/": &fakeResult{
		"Packages",
		[]string{
			"https://golang.org/",
			"https://golang.org/cmd/",
			"https://golang.org/pkg/fmt/",
			"https://golang.org/pkg/os/",
		},
	},
	"https://golang.org/pkg/fmt/": &fakeResult{
		"Package fmt",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
	"https://golang.org/pkg/os/": &fakeResult{
		"Package os",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
}
```

在这段代码中，我们做了以下几件事：
1. 使用一个`map`来记录已经访问过的 URL，以避免重复爬取。
2. 使用`sync.Mutex`来保护对`visited` map 的并发访问，确保线程安全。
3. 创建一个带缓冲的信道`resultChan`来接收爬取结果，并在主协程中打印这些结果。
4. 使用`sync.WaitGroup`来等待所有协程完成爬取任务，并在所有任务完成后关闭信道。
5. 定义一个内部递归函数`crawl`来处理每个 URL 的爬取，并在需要时启动新的协程来处理子 URL。 
