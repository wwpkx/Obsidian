- 大小写敏感的
- 字符串是以Unicode编码
- `#`开头，单行注释；'''包裹，多行注释
- 默认递归深度为100
- not可以将某个条件取反

# Python对象三要素
- id用来唯一标识一个对象
- type标识对象的类型
- value是对象的值
- 判断的是a对象是否就是b对象
	- is，是通过id来判断的
	- `==，是通过value来判断的`

```python
>>> a = 1
>>> b = 1.0
>>> a is b
False
>>> a == b
True
>>> id(a)
12777000
>>> id(b)
14986000

>>> a = 1
>>> b = 1
>>> a is b
True
>>> a == b
True
>>> id(a)
12777000
>>> id(b)
12777000
```

# 数据类型，type()
| 文本类型：  | str                          |
|--------|------------------------------|
| 数值类型：  | int, float, complex          |
| 序列类型：  | list, tuple, range           |
| 映射类型：  | dict                         |
| 集合类型：  | set, frozenset               |
| 布尔类型：  | bool (True 或 False)                        |
| 二进制类型： | bytes, bytearray, memoryview |

| 函数                     | 说明                            |
|------------------------|-------------------------------|
| int(x [,base])        | 将x转换为一个整数                     |
| long(x [,base])       | 将x转换为一个长整数                    |
| hex(x)                 | 将一个整数转换为一个十六进制字符串             |
| oct(x)                 | 将一个整数转换为一个八进制字符串              |
| complex(real [,imag]) | 创建一个复数                        |
| str(x)                 | 将对象 x 转换为字符串                  |
| repr(x)                | 将对象 x 转换为表达式字符串               |
| eval(str)              | 用在字符串中的Python表达式,并返回一个对象 |
| chr(x)                 | 将一个整数转换为一个字符                  |
| unichr(x)              | 将一个整数转换为Unicode字符             |
| ord(x)                 | 将一个字符转换为它的整数值                 |

