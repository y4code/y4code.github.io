+++
date = 2021-01-26T16:00:00Z
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

