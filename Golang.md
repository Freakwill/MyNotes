# Golang

[TOC]

## Examples

```go
// prints its command-line arguments
package main
import ("fmt"
       "os")

func main () {
    var s, sep string
    for i:= 1; i<len(os.Args); i++ {
        s += sep + os.Args[i]
        sep = " "
    }
    fmt.Println(s)
}

func main () {
    var s, sep := "", ""
    for _, arg := range os.Args[1:] {
        s += sep + arg
        sep = " "
    }
    fmt.Println(s)
}

func main () {
    fmt.Println(strings.Join(os.Args[1:], " "))
}

// fmt.Fprintf(format, ...)
```





## Type

### slices

Slice: variable-length seq whose elements all have the same type.

slice op: `s[i:j]`: slice->slice, `array[:]`: array -> slice

built-in function: `make([]int, 3, [cap=10]) // same as make([]int, 10)[:3]`

```go
func equal(x, y []string) bool {
    if len(x) != len(y) {
        return false
    }
    for i := range x {
        if x[i] != y[i] {
            return false
        }
    }
    return true
}
```



### Maps

`map[K]V`

```go
var ages map[string]int
ages := map[string]int {
    "Alice": 31,
    "Charlie": 34
}

ages := make(map[string]int)
ages["Alice"] = 31
...

// iteration
for name, age := range ages {
    ...
}
```



### Struct

The name of a struct field is exported if it begins with a capital letter.



```go
type Point struct {
    X, Y int
}
p:= Point{1,2}  // struct literal

type Circle struct {
    C Point
    R int
}

// point
pp := &Point{1,2} // pp :=new(Point); *pp=Point{1,2}

type tree struct {
    value int
    left, right *tree  // recursive data structure
}

func Sort(values []int) {
    var root *tree
    for _, v := range values{
        root = add(root, v)
    }
    var empty []int
    appendValues(empty, root)
}

func appendValues(values []int, t *tree) []int {
    // [...] -> [...] left value right
    if t != nil {
        values = appendValues(value, t.left)
        values = append(value, t.value)
        values = appendValues(value, t.right)
    }
    return values
}

func add(t *tree, value int) *tree {
    if t==nil {
        return &tree{value:value}
    }
    ...
}
```

#### embeding

```go
type Circle struct {
    Point // anonymous fields
    Radius int
}
```



## Functions

```go
func square(n int) int {return n*n} // square(n int) int {return n*n}
f := square  // func(int) int

strings.Map(func(r rune) rune {return r+1}, "...")  // Anonymous function
```



```go
func f() {
    defer func g() {res...}  // defer function
    ...
    return res
}
```



## Methods

```go
func (p Point) Distance(q Point) float64 {
    // ...
}
p:=Point{2,3}; q:=Point{3,4}
p.Distance(q) // (pp).Distance(q) where pp-=&p

func (p *Point) ScaleBy(factor float64){
    p.X *= factor
    q.Y *= factor
}

(&p).ScaleBy(2) // p == Point{4,6}
```

### Embedding

```go
type Point struct{X, Y float64}
type ColoredPoint struct{
    Point
    Color string
}
p:=ColoredPoint{Point{2,3}, "red"}; q:=ColoredPoint{Point{3,4}, "blue"}
p.Distance(q.Point)  // p.Distance(q) compile materror
```



## Interfaces

"struct of methods"

### Grammar

```go
type Writer interface {
    // declear functions
    Write(p []byte) (n int, err error)
}

type ReadWriter interface {
    Reader(p []byte) (n int, err error)
    Writer   // embedding
}

var w Writer
var rw ReadWriter
// rw satisfies w, w dose not satisfy rw
```



### My works

```go
package main


import (
   "fmt"
)


type Circle struct {
  Radius float64
}

type Show interface {
  show()
}

// method for Circle type
func (c Circle) getArea() float64 {
  return 3 * c.Radius * c.Radius
}

func (c Circle) setRadius(r float64) {
  c.Radius = r
}

func (c Circle) show() {
  fmt.Println("r = ", c.Radius)
  fmt.Println("S = ", c.getArea())
}

func f(a Show) {
  a.show()
}

func main() {
    var c =Circle {Radius: 3}
    c.show()
    f(c)
    var x Show
    x=Circle {Radius: 5}
    f(x)
}
```



## Packages/Files

### Create package  `gopl.io/ch2/tempconv`

- create `gopl.io/ch2/tempconv`

- define types, constants, methods in `tempconv.go`

  ```go
  // DOC
  package tempconv
  import "fmt"
  
  BoilingC=...
  ```

- define functions in `conv.go`

  ```go
  package tempconv
  
  func CToF ...
  ```

- To use it in `gopl.io/ch2/cf`

  ```go
  package mian
  
  import "gopl.io/ch2/tempconv"
  
  fmt.Println(tempconv.CToF(tempconv.BoilingC))
  ```

  ### Initialization

  ```go
  // can't be called, automatically executed when the program starts
  func init() {
      ...
  }
  ```

#### blank imports

`import _ 'temp' // to suppressions the "unused import" error, just call init in temp.go`



### Tools

#### download packages

`go get github...`

go fmt

go doc

### dependence management

godep, vender, gopkg.in

gb