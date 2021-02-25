# 循环



## n.times{ }

### 执行相同操作

```ruby
# 循环指定次数的相同操作
循环次数.times{ 处理}
```

```ruby
5.times{
    p "hello"
}

#终端执行结果：
"hello"
"hello"
"hello"
"hello"
"hello"
```

### 执行不同操作

```ruby
#每一次训话 n 自增+1，开始为0
循环次数.times{|n| 处理}
```

```ruby
sum = 0
5.times{ |n|
    sum += n
    p sum
}

#终端执行结果：
0
1
3
6
10
```



## while do end

```ruby
# 只要满足条件就会循环
# 只要条件表达式值为真，就执行
while 条件 do
    处理
end
```

```ruby
#死循环
while true do 
    p 'hello'
end
```

### 终止循环

```ruby
# 使用全局变量终止循环
n = 0
while n<5 do 
    p 'hello'
    n += 1
end

#终端执行结果：
"hello"
"hello"
"hello"
"hello"
"hello"
```

```ruby
# 使用break终止需循环
n = 0
while true do 
    n += 1
    break if n==3 
end 
p n

#终端执行结果：
3
```



### begin end while

```ruby
# 仅会执行一次
# 先执行后判断while的条件
begin
    处理
end while 条件
```

```ruby
n = 0
while n>0 do 
    p 'hello'
end
# 条件为false，不会执行

n = 0
begin
    p 'hello'
end while n>0

#终端执行结果：
"hello"
```



## until do end 

```ruby
# 会一直循环，直到满足条件
# 只要条件表达式结果是假，就执行循环
```

```ruby
# 死循环
until false do 
    p "hello"
end
```

```ruby
arr = [1,2,3,4]
until arr.size<3 do 
    arr.delete_at(-1)
end
p arr

#终端执行结果：
[1, 2]
```



## loop do end

```ruby
# 会一直循环
loop do 
    处理
end
```

```ruby
# 死循环
loop do 
    p "hello"
end
```

### 终止循环

```ruby
# 使用break终止循环
n = 0
loop do 
    n += 1
    break if n==3 
end 
p n

#终端执行结果：
3
```

### 和while的不同

```ruby
# 和while不同的是，
# while块级作用域内的变量，可以在作用域外调用
# loop块级作用域内的变量，不能在作用域外调用，会报错
```

```ruby
# while内给声明并赋值变量a
# a在while结束后也可以被调用
n = 0
while true do 
    n += 1
    a = 100
    break if n==3 
end 
p a
#终端执行结果：
100

# loop内给声明并赋值变量a
# a在loop结束后不能被调用
n = 0
loop do 
    n += 1
    a = 100
    break if n==3 
end 
p a
#终端执行结果：
Traceback (most recent call last):
        1: from 01.rb:2:in `<main>'
```



## for

```ruby
for 变量 in 目标 do 
    处理
end
```

```ruby
arr = [1,2,3,4]
sum = 0

for n in arr do
    sum += n
end
p sum

#终端执行结果：
10
```

```ruby
# Ruby中更常用 each，写起来更简洁
arr = [1,2,3,4]
sum = 0

arr.each{|v|
    sum += v
}
p sum

#终端执行结果：
10
```



## a.upto(b)

```ruby
# 上升
# a和b都必须是整数integer
# 从a开始到b，逐一增加
```

### 执行同一操作

```ruby
数字1.upto(数字2){ 处理 }
# 重复次数是 数字1~数字2，包含数字1和数字2
```

```ruby
0.upto(3){ 
    p "hello"
}

#终端执行结果：
"hello"
"hello"
"hello"
"hello"
```

### 执行不同操作

```ruby
数字1.upto(数字2){ |item| 处理 }
```

```ruby
0.upto(3){ |v|
    p v
}

#终端执行结果：
0
1
2
3
```

```ruby
arr = []
0.upto(3){ |v|
    arr << v
}
p arr

#终端执行结果：
[0, 1, 2, 3]
```



## a.downto(b)

```ruby
# 下降
# a和b都必须是整数integer
# 从a开始到b，逐一增加
```

### 执行同一操作

```ruby
数字1.downto(数字2){ 处理 }
# 重复次数是 数字1~数字2，包含数字1和数字2
```

```ruby
0.downto(3){ 
    p "hello"
}

#终端执行结果：
"hello"
"hello"
"hello"
"hello"
```

### 执行不同操作

```ruby
数字1.upto(数字2){ |item| 处理 }
```

```ruby
0.downto(3){ |v|
    p v
}

#终端执行结果：
3
2
1
0
```

```ruby
arr = []
3.downto(0){ |v|
    arr << v
}
p arr
#终端执行结果：
[3, 2, 1, 0]
```



## .step( ){ }

```ruby
开始值.step(结束值，间隔值){|item| 处理}
```

```ruby
# 从1开始到10循环，中间每个item间隔2
1.step(10,2){|n|
    p n 
}

#终端返回的结果：
1
3
5
7
9
```

```ruby
#可以从高到低递减
```

```ruby
10.step(1,-2){|n|
    p n 
}

#终端返回的结果：
10
8
6
4
2
```





## break关键字

```ruby
# 终止全部循环，退出循环
```

## redo关键字

```ruby
# 重新开始循环
```

## next 关键字

```ruby
# 终止当前循环，进行循环内的下一步
```

```ruby

```

