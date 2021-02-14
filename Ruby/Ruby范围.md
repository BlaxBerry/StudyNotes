# 创建范围

## a..b

```ruby
# 从a~b,包含b
1..3
a..d
```

```ruby
a = 1..5
p a.min()
p a.max()

#终端返回的结果：
1
5
```

```ruby
arr = []
(1..5).each{|v|
    p v 
    arr << v
}
p arr

#终端返回的结果：
1
2
3
4
5
[1, 2, 3, 4, 5]
```



## a...b

```ruby
# 从a~b,不包含b
1..3
a..d
```

```ruby
a = 1...5
p a.min()
p a.max()

#终端返回的结果：
1
4
```

```ruby
res = []
score = [70,60,40,90]
score.each{|v|
    case v
    when 80..100
        p "优秀"
        res << "优秀"
    when 60...80
        p "一般"
        res << "一般"
    else 
        p "差"
        res << "差"
    end
}
p score
p res

#终端返回的结果：
[70, 60, 40, 90]
["一般", "一般", "差", "优秀"]
```



## Range.new( )

```ruby
变量 = Range.new(范围起始，范围终结)
```

```ruby
range = Range.new(1,10)
p range

#终端返回的结果：
1..10
```





# 范围中是否包含某值

## range.include?( )

```ruby
范围.include?(值)
# 返回true /fasle
```

```ruby
range = Range.new(1,10)

p range.include?(5)
p range.include?(20)

#终端返回的结果：
true
false
```



## range === value

```ruby
# 判断范围里是否有值
范围 === 值
```

```ruby
range = Range.new(1,10)
p range === 10
p range === 20

p (1...10)===10
p (1..10)===10

#终端返回的结果：
true
false
false
true
```





# 范围长度

## range.size

```ruby
# 获得范围中值的个数
```

```ruby
range = Range.new(1,10)
p range.size
p (1...10).size

#终端返回的结果：
10
9
```



# 范围的最值

## range.max

```ruby
# 获得范围中的最大值（截至值）
```

```ruby
range = Range.new(1,10)
p range.max
p (1...10).max

#终端返回的结果：
10
9
```



## rang.min

```ruby
# 获得范围中的最大值（起始值）
```

```ruby
range = Range.new(1,10)
p range.min
p (1...10).min

#终端返回的结果：
1
1
```

