* An untyped constant takes the type needed by its context.
```go
package main

import "fmt"

const (
  // Create a huge number by shifting a 1 bit left 100 places.
  // In other words, the binary number that is 1 followed by 100 zeroes.
  Big = 1 << 100
  // Shift it right again 99 places, so we end up with 1<<1, or 2.
  Small = Big >> 99
)

func needInt(x int) int { return x*10 + 1 }
func needFloat(x float64) float64 {
  return x * 0.1
}

func main() {
  fmt.Println(needInt(Small))
  fmt.Println(needFloat(Small))
  fmt.Println(needFloat(Big))
}
```
Go will check for overflow, fmt.Println(needInt(Big)) will throw exception.

* struct in Go:

```go
type Vertex struct {
  X int
  Y int
}


func main() {
  fmt.Println(Vertex{1, 2})
}
```

* map
make(map[string]int) will create a map with string keys and int values.
elem, ok := map[key], elem will be the value, if ok is false, then elem will
be the default value of type key.


* function values
Function could be closure, this is a difference between functional language's 
function and C's function pointer.

* Type Switch
interface{} is just like object in other language, but it is a tuple. i.(type)
used in a switch will get the type of the interface i.


* Stringers
One of the most common interface is Stringer defined by the fmt package.

type Stringer interface {
  String() string
}

A Stringer is a type that can describe itself as a string. The fmt package look
for this interface to print values.

fmt.Sprintf() could format a string.


* if-else

You have to write

if xxx {
} else {
}

instead of if xx {
}
else {
}

it will lead to a runtime error.


* My excercise for image:
package main

import (
  "golang.org/x/tour/pic"
  "image"
  "image/color"
)

type Image struct{}

func (im Image) ColorModel() color.Model {
  return color.RGBAModel
}

func (im Image) Bounds() image.Rectangle {
  return image.Rect(0, 0, 256, 256)
}

func (im Image) At(x, y int) color.Color {
  v := uint8(x^y)
  return color.RGBA{v, v, 255, 255}
}

func main() {
  m := Image{}
  pic.ShowImage(m)
}


* Fibonacci 
This is interesting, the evaluate model.

package main

import "fmt"

// fibonacci is a function that returns
// a function that returns an int.
func fibonacci() func() int {
  i, j := 0, 1
  return func() int {
    res := i
    i, j = j, i + j
    return res
  }
}

func main() {
  f := fibonacci()
  for i := 0; i < 10; i++ {
    fmt.Println(f())
  }
}


* Concurrence

** channel
Let c := make(chan int, 10)
The loop for i := range c receives values from the channel repeatedly until it is closed.

** Web Crawler

package main

import (
  "fmt"
)

type Fetcher interface {
  // Fetch returns the body of URL and
  // a slice of URLs found on that page.
  Fetch(url string) (body string, urls []string, err error)
}

type Result struct {
  url, body string
  urls      []string
  err       error
  depth   int
}

func _fetch(url string, fetcher Fetcher, res chan Result, depth int) {
  body, urls, err := fetcher.Fetch(url)
  res <- Result{url: url, body: body, urls: urls, err: err, depth: depth}
}

func Crawl(url string, depth int, fetcher Fetcher) {
  res := make(chan Result)
  cache := make(map[string]bool)
  results := make([]Result, 0)

  if depth <= 0 {
    return
  }
  go _fetch(url, fetcher, res, depth)
  cache[url] = true
  r := <-res
  results = append(results, r)
  for len(results) > 0 {
    r = results[0]
    results = results[1:]
    if r.err == nil {
      fmt.Printf("found: %s %q\n", r.url, r.body)
    } else {
      fmt.Println(r.err)
    }
    if r.depth > 0 && r.err == nil {
      for _, u := range r.urls {
        if !cache[u] {
          go _fetch(u, fetcher, res, r.depth-1)
          r = <-res
          results = append(results, r)
          cache[u] = true
        }
      }
    }
  }
  return
}

func main() {
  Crawl("http://golang.org/", 4, fetcher)
  //  go Crawl("http://golang.org/pkg/", 4, fetcher)
}

// fakeFetcher is Fetcher that returns canned results.
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

// fetcher is a populated fakeFetcher.
var fetcher = fakeFetcher{
  "http://golang.org/": &fakeResult{
    "The Go Programming Language",
    []string{
      "http://golang.org/pkg/",
      "http://golang.org/cmd/",
    },
  },
  "http://golang.org/pkg/": &fakeResult{
    "Packages",
    []string{
      "http://golang.org/",
      "http://golang.org/cmd/",
      "http://golang.org/pkg/fmt/",
      "http://golang.org/pkg/os/",
    },
  },
  "http://golang.org/pkg/fmt/": &fakeResult{
    "Package fmt",
    []string{
      "http://golang.org/",
      "http://golang.org/pkg/",
    },
  },
  "http://golang.org/pkg/os/": &fakeResult{
    "Package os",
    []string{
      "http://golang.org/",
      "http://golang.org/pkg/",
    },
  },
}

* xml serialization
encode/xml. First new encoder, then set indent.One thing need to note is the private fields will not be marshalled.
