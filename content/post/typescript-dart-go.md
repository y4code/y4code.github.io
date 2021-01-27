+++
date = 2021-01-26T16:00:00Z
title = "用 TypeScript/Dart 来写 Go 的接收者"

+++
# 用 TypeScript/Dart 来写 Go 的接收者

Write Receiver of Go in TypeScript/Dart Way

# Go 接收者

```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}
```

# TypeScript

```typescript
class Vertex {
	public x: number
	public y: number
	constructor(x: number, y: number) {
		this.x = x;
		this.y = y;
	}
	Abs(): number {
		return Math.sqrt(this.x * this.x + this.y * this.y);
	}
}

type x = Number;

const v = new Vertex(3, 4);
console.log(v.Abs());
```

# Dart

```dart
import 'dart:math';

class Vertext {
  final double x;
  final double y;
  Vertext(this.x, this.y);
  double Abs() {
	return sqrt(this.x * this.x + this.y * this.y);
  }
}

main() {
  var v = Vertext(3, 4);
  print(v.Abs());
}
```

