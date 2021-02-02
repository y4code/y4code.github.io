+++
date = 2021-02-01T16:00:00Z
title = "ASCII 与 Unicode 与 UTF-8"

+++
# ASCII 与 Unicode 与 UTF-8

```go
var ch int = '\u0041'
var ch2 int = '\u03B2'
var ch3 int = '\U00101234'
fmt.Printf("%d - %d - %d\n", ch, ch2, ch3) // integer
fmt.Printf("%c - %c - %c\n", ch, ch2, ch3) // character
fmt.Printf("%X - %X - %X\n", ch, ch2, ch3) // UTF-8 bytes
fmt.Printf("%U - %U - %U\n", ch, ch2, ch3) // UTF-8 code point
```

# `\u0041` 是什么意思

第一句将 ch 声明为整形，但是却用 Unicode 的方法来表示这个整数，它与 `var ch int = 65` 完全一致，`\u0041`与`\u03B2` 为 Unicode 码点，后面的数字为16进制表示，计算 4*16+1 刚好等于 65

# ASCII 是什么

只有128 个的数字码与字符的对应规则，eg: 大写字母 A 对应的码为 65（十进制）或0x41（十六进制）

# Unicode 是什么

万国码，每一种语言的每一个文字都有一个唯一的码对应，eg：大写字母 A 对应的码为 `U+0041` , 汉字「尸」对应的 Unicode 码为 `U+5C38`

# UTF-8/UTF-16/UTF-32  是什么

不定存储长度的基于 Unicode 的编码规则，此编码规则的开始的一小段（0-127）和ASCII 码相同，即 65-90位为大写英文字母 A-Z，97-122 位为a-z，eg：大写字母A在 UTF-8 编码规则下表示为 `0x41` , 在 UTF-16 编码规则下表示为 `0x0041` , 在 UTF-32 编码规则下表示为 `0x00000041`

# 为什么 UTF-8 编码规则下的0x后面是两位数字

UTF-8 即用 8 位二进制数字（eg: 10001000）来表示字符，在最大只能容纳 8 位的限制下，其能表示的最大数字也是两位十六进制数字可以表示的最大值，也就是说 11111111 ( 二进制 ) = 0xFF ( 十六进制 )

UTF-16/UTF-32 同理