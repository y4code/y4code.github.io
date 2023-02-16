+++
date = 2021-01-26T16:00:00.000Z
title = "读 Effective Go 笔记"

+++
# Go Code Snippet 「Effective Go 」

# 正则查找go doc

```shell
go doc -all regexp | grep -i parse
```

# 重复声明 :=

在满足下列条件时，已被声明的变量 v 可出现在:= 声明中：

- 本次声明与已声明的 v 处于同一作用域中（若 v 已在外层作用域中声明过，则此次声明会创建一个新的变量 §），
- 在初始化中与其类型相应的值才能赋予 v，且
- 在此次声明中至少另有一个变量是新声明的。

这个特性简直就是纯粹的实用主义体现，它使得我们可以很方便地只使用一个 err 值，例如，在一个相当长的 if-else 语句链中， 你会发现它用得很频繁。

# 遍历数组、切片、字符串或者映射

若你想遍历数组、切片、字符串或者映射，或从信道中读取消息， range 子句能够帮你轻松实现循环。

```go
for key, value := range oldMap {
	newMap[key] = value
}
```

# Switch 做判断

```go
func unhex(c byte) byte {
	switch {
	case '0' <= c && c <= '9':
		return c - '0'
	case 'a' <= c && c <= 'f':
		return c - 'a' + 10
	case 'A' <= c && c <= 'F':
		return c - 'A' + 10
	}
	return 0
}
```

`switch` 并不会自动下溯，但 `case` 可通过逗号分隔来列举相同的处理条件。

```go
func shouldEscape(c byte) bool {
	switch c {
	case ' ', '?', '&', '=', '#', '+', '%':
		return true
	}
	return false
}
```

# Switch 做类型选择

```go
var t interface{}
t = functionOfSomeType()
switch t := t.(type) {
default:
	fmt.Printf("unexpected type %T\n", t)	 // %T 打印任何类型的 t
case bool:
	fmt.Printf("boolean %t\n", t)			 // t 是 bool 类型
case int:
	fmt.Printf("integer %d\n", t)			 // t 是 int 类型
case *bool:
	fmt.Printf("pointer to boolean %t\n", *t) // t 是 *bool 类型
case *int:
	fmt.Printf("pointer to integer %d\n", *t) // t 是 *int 类型
}
```

# 命名结果参数

```go
func ReadFull(r Reader, buf []byte) (n int, err error) {
	for len(buf) > 0 && err == nil {
		var nr int
		nr, err = r.Read(buf)
		n += nr
		buf = buf[nr:]
	}
	return
}
```

# 空接口

`fmt.Print` 可接受类型为 `interface{}` 的任意数量的参数。

# 使用 make 除非要获得明确指针再使用new()

> `make` 只适用于映射、切片和信道且不返回指针。若要获得明确的指针， 请使用 `new` 分配内存。

make

```go
return &File{fd, name, nil, 0}
```

```go
return &File{fd: fd, name: name}
```

```go
v := make([]int, 100)
```

new

```go
type SyncedBuffer struct {
	lock	sync.Mutex
	buffer  bytes.Buffer
}

// SyncedBuffer 类型的值也是在声明时就分配好内存就绪了。后续代码中， p 和 v 无需进一步处理即可正确工作。

var v SyncedBuffer	  // type  SyncedBuffer
p := new(SyncedBuffer)  // type *SyncedBuffer
```

# 把一维数组变成二维数组的Trick

```go
picture := make([][]uint8, YSize) //  每 y 个单元一行。
// 分配一个大一些的切片以容纳所有的元素
pixels := make([]uint8, XSize*YSize) // 指定类型[]uint8, 即便图片是 [][]uint8.
//循环遍历图片所有行，从剩余像素切片的前面对每一行进行切片。
for i := range picture {
	picture[i], pixels = pixels[:XSize], pixels[XSize:]
}
```

# 转换类型以使用其方法集合

要实现排序功能，没有必要「实现排序方法」，只需要位于实现 `src/sort/sort.go` 名字为 `Interface` 的接口里的三个方法即可，这三个方法是:

```go
type Interface interface {
	// Len is the number of elements in the collection.
	Len() int
	// Less reports whether the element with
	// index i should sort before the element with index j.
	Less(i, j int) bool
	// Swap swaps the elements with indexes i and j.
	Swap(i, j int)
}
```

然后对一个类型为 `Sequence` 的变量 `s` 进行排序时即可

```go
sort.Sort(s)
```

或

```go
sort.IntSlice(s).Sort()
```

# 实现个某个接口即类型等于该接口类型

```go
type Stringer interface {
	String() string
}

var value interface{} // Value 由调用者提供
switch str := value.(type) {
case string:
	return str
case Stringer:
	return str.String()
}
```

# 类型断言

```go
if str, ok := value.(string); ok {
	return str
} else if str, ok := value.(Stringer); ok {
	return str.String()
}
```

# Channel 作为接受者

```go
// 每次浏览该信道都会发送一个提醒。
// （可能需要带缓冲的信道。）
type Chan chan *http.Request

func (ch Chan) ServeHTTP(w http.ResponseWriter, req *http.Request) {
	ch <- req
	fmt.Fprint(w, "notification sent")
}
```

换一种思路，这是一种优雅的发送通知的方式

# 函数作为接受者即函数实现了接口

```go
// HandlerFunc 类型是一个适配器，
// 它允许将普通函数用做HTTP处理程序。
// 若 f 是个具有适当签名的函数，
// HandlerFunc(f) 就是个调用 f 的处理程序对象。
type HandlerFunc func(ResponseWriter, *Request)

// ServeHTTP calls f(w, req).
func (f HandlerFunc) ServeHTTP(w ResponseWriter, req *Request) {
	f(w, req)
}
```

类似于 `Sequence` 转成 `IntSlice` 以访问 `IntSlice.Sort` 一样， 这里把 `ArgsServer` 类型转成 `HandlerFunc`  以调用 `ServeHTTP` , tmd 绝了

```go
http.Handle("/args", http.HandlerFunc(ArgServer))
```

一切都是因为接口只是方法（Method）的集合，而几乎任何类型都能定义方法（Method），包括函数（Function）

# 一种极其罕见的显示接口声明方法

```go
var _ json.Marshaler = (*json.RawMessage)(nil)
```

# 内嵌（Embedding）

```go
type Job struct {
	Command string
	*log.Logger
}
```

现在 `*log.Logger` 上的方法可以直接通过 `Job` 访问到，即 `Job.Logger.Println 可以直接通过 Job.Println` 访问

1. 此处没有字段名时，结构名即为字段名，`Job.Logger`
2. 在上一条规则的前提下，内嵌时，不能存在同名字段名，且外部类型的访问字段可以覆盖内部类型的访问字段，你可以理解为面向对象编程中「重写 Override」

> Correct

```go
type Job struct {
	Command string
	*log.Logger
}
```

or

```go
type Job struct {
	Command string
	Logger string
}
```

> Wrong

```go
type Job struct {
	Command string
	Logger string
	*log.Logger
}
```

# Channel 自带阻塞机制

```go
c := make(chan int)  // 创建一个无缓冲的类型为整型的 channel
//用 goroutine 开始排序；当它完成时，会在信道上发信号。
go func() {
	list.Sort()
	c <- 1  // 发送一个信号，这个值并没有具体意义
}()
doSomethingForAWhile()
<-c   // 等待 sort 执行完成，然后从 channel 取值
```

# Channel 写法的理解

```go
for {
	go func() {
		// do something
	}()
}
```

`go func` 放在 `for` 循环里面的原因是保证协程不会被杀死，在执行完上一个协程之后紧接着执行下一个协程；如果不这样做，主程序执行完 `go func` 这条命令后就停止了，附着在主程序里的协程也会被终止，所以 `for` 循环搭配 `go func` 是惯常用法（idiomatic expression/ convention）

# 由闭包引发的产生循环变量的两种方式

第一种是在 `for` 循环中使用索引取值

```go
func main() {
	var out []*int
	for i := 0; i < 3; i++ {
		out = append(out, &i)
	}
	fmt.Println("Values:", *out[0], *out[1], *out[2])
	fmt.Println("Addresses:", out[0], out[1], out[2])
}
```

第二种是在 `for` 循环中的 `go func` 中引用索引

```go
for _, val := range values {
	go func() {
		fmt.Println(val)
	}()
}
```

# 如何避免

第一种的避免方式

```go
for i := 0; i < 3; i++ {
+	i := i // Copy i into a new variable.
 	out = append(out, &i)
 }
```

第二种的避免方式

```go
for _, val := range values {
	go func(val interface{}) {
		fmt.Println(val)
	}(val)
}
```

需要注意的一点是如果没有 `go` 关键字，只是一个subfunction，那么就不会造成这个问题

```go
for i := 1; i <= 10; i++ {
	func() {
		fmt.Println(i)
	}()
}
// 1
// 2
// 3
// ...
```

[CommonMistakes](https://github.com/golang/go/wiki/CommonMistakes)

