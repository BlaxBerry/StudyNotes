# 创建数组

##  [ ]

```ruby 
# 创建空数组
arr = []
# 声明并赋值
arr = [元素，元素，...]
```



## Array.new( )

```ruby 
# 创建空数组
变量 = Array()
```

```ruby
a = Array.new()
p a
p a[0]
p a.size

# 终端显示结果：
[]
nil
0
```

```ruby
# 带一个参数的话数组长度，
# 此时还是一个空数组，元素是空值nil
变量 = Array.new(number)
```

```ruby
a = Array.new(5)
p a.size
p a

# 终端显示结果：
5
[nil, nil, nil, nil, nil]
```

```ruby
# 带两个参数的话，是长度个值
# 赋的值都是相同的
变量 = Array.new(size,item)
```

```ruby 
a = Array.new(5,"A")
p a

b = Array.new(5,nil)
p b

# 终端显示结果：
["A", "A", "A", "A", "A"]
[nil, nil, nil, nil, nil]
```



## Array.new( ){  }

```ruby 
# 赋不同的值时：
变量 = Array.new(size){ |item| 处理}
```

```ruby 
num = 0
a = Array.new(5){ |v| "A#{num+=1}"}
p a

# 终端显示结果：
["A1", "A2", "A3", "A4", "A5"]
```



## %w!item item ...!

```ruby
# 也是一种声明数组的方式，Ruby2.8版本新增
# 元素之间用空格隔开
# 元素全都是字符串
%w!元素 元素 ...!
```

```ruby
a = %w!A B C!
p a

b = %w!1 2 3!
p b

c = %w!true false nil!
p c

# 终端显示结果：
["A", "B", "c"]
["1", "2", "3"]
["true", "false", "nil"]
```

```ruby
# 字符串元素值的空格使用 转义字符 \空格
%w!item_content\ item_content ...!
```

```ruby
a = %w!hello\ Ruby hell\ python!
p a

# 终端显示结果：
["hello Ruby", "hell python"]
```



## %w!(item item ...)

```ruby
# 也是一种声明数组的方式,Ruby2.8版本新增
# 和用! !括住一样，只不过是()代替了!!
# 元素之间用空格隔开
# 元素全都是字符串
%w(元素 元素 ...)
```

```ruby
a = %w(A B c)
p a

b = %w(1 2 3)
p b

c = %w(true false nil)
p c

# 终端显示结果：
["A", "B", "c"]
["1", "2", "3"]
["true", "false", "nil"]
```

```ruby
# 字符串元素的空格使用 转义字符 \空格
%w(item_content\ item_content ...)
```

```ruby
a = %w(hello\ Ruby hell\ python)
p a

# 终端显示结果：
["hello Ruby", "hell python"]
```





# 转换为数组

## str.chars

```ruby
# 把字符串的的每一个字符都拆成数组元素
# 返回一个数组
```

```ruby
a = "Hello Ruby"
p a.chars

#终端返回的结果：
["H", "e", "l", "l", "o", " ", "R", "u", "b", "y"]
```



### str.split( )

```ruby
# 参数是分隔符
string.split(分隔符)
```

```ruby
b = "Hello#Ruby"
p b.split("#")

#终端返回的结果
["Hello", "Ruby"]
```

```ruby
# 参数不写，是默认去空格或逗号
# 其余分隔符必须输入参数
```

```ruby
a = "Hello Ruby"
p a.split
p a.split()

b = "Hello,Ruby"
p b.split
p b.split()

#终端返回的结果：
["Hello", "Ruby"]
["Hello", "Ruby"]

["Hello", "Ruby"]
["Hello", "Ruby"]
```



# 转换为字符串

## arr.join( )

```ruby
# 可以添加字符串的参数作为连接符
array.join(字符串)
```

```ruby
a = [1,2,3]
p a.join()
p a.join(',')
p a.join('#')

#终端执行结果：
"123"
"1,2,3"
"1#2#3"
```

```ruby
# 参数也可以写文本
```

```ruby
#修改元素，并用and连接，并转为字符串
names = ["Anderson","James","Davis"]

names_Mr = names.map{|v| "Mr.#{v}"}.join(" and ")
p names_Mr

#终端执行结果：
"Mr.Anderson and Mr.James and Mr.Davis"
```



# 获得元素

## arr [index]

```ruby 
#从前开始开始计算，序号从0开始
arr[正数]
```

```ruby 
a = [1,2,3]
p a[0]
p a[1]
p a[2]

#终端返回的结果：
1
2
3
```

## arr [-index]

```ruby 
#从后开始开始计算，序号从-1开始
arr[负数]
```

```ruby 
a = [1, 2, 3, 4]
p a[-1]
p a[-2]
p a[-0]  #-0还是0

#终端返回的结果：
4
3
1
```



## arr[index , number]

```ruby 
#从第几个序号处开始向后获取几个元素
arr[从哪开始取 ,取几个 ]
```

```ruby 
a = [1,2,3,4,5]
p a[0, 2]
p a[1, 3]

p a[-2 ,2]
p a[-1, 2] # -1是5，5后面已经没有元素了，只能取到一个5

#终端返回的结果：
[1, 2]
[2, 3, 4]
[4, 5]
[5]
```



## arr.value_at( index )

```ruby 
# 返回数组中指定序号的元素，以数组形式返回
arr.value_at(index)

# 返回数组中指定序号的多个元素，以数组形式返回
arr.value_at(index，index，...)
```

```ruby 
a = [1,2,'A','B',5]
p a.values_at(0)
p a.values_at(1)

p a.values_at(2,3,4)

#终端返回的结果：
[1]
[2]
["A", "B", 5]
```



## arr.first( )

```ruby 
#返回数组的从前面起的第一个元素
arr.first

#返回数组的从前面起的第一个~第n个元素，并放入一个数组
arr.first(参数)
```

```ruby 
a = [1,2,3,4,5]
p a.first
p a.first()

p a.first(1)
p a.first(2)

#终端返回的结果：
1
1
[1]
[1, 2]
```



## arr.last( )

```ruby 
#返回数组的倒数第一个元素
arr.last

#返回数组的从后面起的第n个~倒数第一个元素，并放入一个数组
arr.larst(参数)
```

```ruby 
a = [1,2,3,4,5]
p a.last
p a.last()

p a.last(1)
p a.last(3)

#终端返回的结果：
5
5
[5]
[3, 4, 5]
```



## arr.find do end

```ruby
# 返回数组中第一个符合条件的（条件表达式值为true的）元素
array.find do |item| 
    条件
end
```

```ruby
arr = [1,2,3,4,5,6]
target_item = arr.find do |v| 
    v.even?
end
p target_item
p arr

#终端返回的结果：
2
[1, 2, 3, 4, 5, 6]
```



## arr.find{ } / arr.detect{ }

```ruby
# 用块级作用域思想
# 和.find do end 一样
# 返回数组中第一个符合条件的（条件表达式值为true的）元素
array.find{ |item| 条件}
```

```ruby
arr = [1,2,3,4,5,6]

target_item = arr.find{ |v| v.even?} 
p target_item
p arr

#终端返回的结果：
2
[1, 2, 3, 4, 5, 6]
```



## arr.select{ } / arr.find_all{ }

```ruby
# 返回所有符合条件的（条件表达式值为true的）元素
# 返回一个数组
array.select{ |item| 条件}
```

```ruby
arr = [1,2,3,4,5,6]

arr_new = arr.select{ |v| v.even?} 
p arr_new
p arr

#终端返回的结果：
[2, 4, 6]
[1, 2, 3, 4, 5, 6]
```





# 获取数组长度

## arr.size / arr.length

```ruby 
# 即获取元素的个数
# 数组序号是从0开始，最后一个元素的序号是 arr.size - 1

# .size 或 .length
```

```ruby 
a = [1,2,3,4,5]
p a.size
p a.length

p a[a.size - 1]
p a[a.length -1]

#终端返回的结果：
5
5
5
5
```





# 修改数组元素值

## arr[index] = value

```ruby 
#若数组中有该序号，则是修改
a = [1,2,3]
a[0] = "A"
a[2] = "C"
p a

#终端返回的结果：
["A", 2, "C"]
```

```ruby 
#若没有该序号的元素则是追加
a = [1,2,3]
a[3] = 4
a[4] = 5
p a

#终端返回的结果：
[1, 2, 3, 4, 5]
```



## arr[index，number] = value

```ruby 
# 从指定序号开始后的n个元素的值都修改为指定值
# 并对返回结果进行排重
```

```ruby 
a = [1,2,3,4]
a[0,1] = 'A'
a[2,3] = 'B'
p a

#终端返回的结果：
["A", 2, "B"]
```



## arr.map do end

```ruby
# 遍历数组，修改数组的元素
# 修改后返回一个新数组，不影响原数组
array.map do |item|
    处理
end
```

```ruby
arr = [1,2,3]
arr_new = arr.map do |v|
    v *= 10
end
p arr_new
p arr

#终端返回的结果：
[10, 20, 30]
[1, 2, 3]
```

```ruby
arr = [1,2,3]

arr_new = arr.map do |v|
   v.odd? ? v*10 : v
end
p arr_new
p arr

#终端返回的结果：
[10, 2, 30]
[1, 2, 3]
```



## arr.map{ } / arr.collect{ }

```ruby
# 用块级作用域思想
# 和.map do end 一样
# 遍历数组，修改数组的元素
# 修改后返回一个新数组，不影响原数组
array.map{ |item| 处理 }
```

```ruby
arr = [1,2,3]

arr_new = arr.map{ |v| v * 10 }
p arr_new
p arr

#终端返回的结果：
[10, 20, 30]
[1, 2, 3]
```

```ruby
arr = [1,2,3]

arr_new = arr.map{ |v| v.odd ? v*10 : v }
p arr_new
p arr

#终端返回的结果：
[10, 2, 30]
[1, 2, 3]
```





# 追加元素

## arr.unshift

```ruby
# 向数组中追加元素,加到数组最前面
arr.unshift(item,item,item,... )
```

```ruby
arr = [1,2,3]

arr.unshift(0)
p arr

arr.unshift("A","B")
p arr

#终端返回的结果：
[0, 1, 2, 3]
["A", "B", 0, 1, 2, 3]
```



## arr.push( )

```ruby 
# 向数组中追加元素,加到数组最末尾
arr.push(item,item,item,... )
# 比起 << 可以一次追加多个元素
```

```ruby 
a = [1,2,3]
a.push('A')
a.push('B','B')
p a

#终端返回的结果：
[1, 2, 3, "A", "B", "B"]
```

```ruby 
# 也可以采用链式编程
a = [1,2,3]
a.push('A').push('B','B')
p a

#终端返回的结果：
[1, 2, 3, "A", "B", "B"]
```



## arr[index] = value

```ruby 
#若没有该序号的元素则是追加
a = [1,2,3]
a[3] = 4
a[4] = 5
p a

#终端返回的结果：
[1, 2, 3, 4, 5]
```



## arr << item

```ruby
# 把元素代入数组
数组 << 元素 << 元素
```

```ruby
arr = [1,2,3]
arr << 4
p arr

arr << 5 << 6
p arr

#终端返回的结果：
[1, 2, 3, 4]
[1, 2, 3, 4, 5, 6]
```



## arr.insert()

```ruby
# 向数组的指定位置的前面插入一个指定值
arr.insert(指定位置序号，值)
```

```ruby
arr = [1,2,3]

arr.insert(2,"A")
p arr

#终端返回的结果：
[1, 2, "A", 3]
```





# 删除元素

## arr.shift

```ruby
# 删除数组的第一个元素
数组.shift
```

```ruby
arr = [1,2,3,4,5]

arr.shift
p arr

#终端返回的结果：
[2, 3, 4, 5]
```



## arr.pop

```ruby
# 删除数组的最后一个元素
数组.pop(item，item,...)
```

```ruby
arr = [1,2,3,4,5]

arr.pop
p arr

#终端返回的结果：
[1, 2, 3, 4]
```





## arr.delete( )

```ruby 
#从数组中删除指定元素
```

```ruby 
a = [1,'A','B',5]
a.delete('A')
a.delete('B')
p a

#终端返回的结果：
[1, 5]
```

```ruby 
#一次只能删除一个，不然会报错
a = [1,'A','B',5]
a.delete('A','B')
p a

#终端返回的结果：
Traceback (most recent call last):
        1: from 01.rb:2:in `<main>'
01.rb:2:in `delete': wrong number of arguments (given 2, expected 1) (ArgumentError)
```



## arr.delete_at( index )

```ruby 
# 删除数组中指定位置的元素
# 返回值时删除的元素
```

```ruby 
a = ["A","B","C"]
a.delete_at(0)
p a

p a.delete_at(0)

#终端返回的结果：
["B", "C"]
["A"]
```

```ruby 
# 若删除的序号不存在，不会执行操作
a = ["A","B","C"]
a.delete_at(9)
p a

p a.delete_at(9)

#终端返回的结果：
["A", "B", "C"]
nil
```



## arr.delete_if do end

```ruby
# 循环遍历数组，判断元素是否满足条件，若满足就删除元素
```

```ruby
arr.delete_if do |v|
    v 判断条件?
end

#就是each嵌套if的简写
arr.each do |v|
    if 判断条件
        arr.delete(v)
    end
end
```

```ruby
# 例子：删除数组中的奇数元素
a = [1,2,3,4,5,6]
a.delete_if do |v|
    v.odd?
end
p a

#终端返回的结果：
[2, 4, 6]


# 其实就是如下：
arr = [1,2,3,4,5,6]

arr.each do |v|
    if v .odd?
        arr.delete(v)
    end
end
p arr

#终端返回的结果：
[2, 4, 6]
```

```ruby
arr = [1,2,3,4,5]

arr.delete_if do |v|
   v == 3
end
p arr

#终端返回的结果：
[1,2,4,5]
```

## arr.delete_if{ }

```ruby
# 用块级作用域的思想
# 和array.delete_if do end 一样
# 循环遍历数组，判断元素是否满足条件，若满足就删除元素
```

```ruby
array.delete_if {|item| 条件 }
```

```ruby
# 例子：删除数组中的奇数元素
a = [1,2,3,4,5,6]
a.delete_if { |v| v.odd? }
p a

#终端返回的结果：
[2, 4, 6]
```



## arr.reject{ }

```ruby
# 循环遍历数组，判断元素是否满足条件，若满足就删除元素
# 删除条件表达式值为true的所有符合元素
# 返回一个新数组

# 和delete_if{}类似
```

```ruby
array.reject{ |item| 条件}
```

```ruby
arr = [1,2,3,4,5,6]
arr_new =arr.reject { |v| v.odd? }
p arr_new
p arr

#终端返回的结果：
[2, 4, 6]
[1, 2, 3, 4, 5, 6]
```

```ruby

```



# 判断是否包含元素

## arr.include?( )

```ruby
# 判断数组中是否含有指定元素
arr.include?(元素)
# 返回值是true/false
```

```ruby
a = [1,2,3,4]
p a.include?(2)
p a.include?(9)

#终端返回的结果：
true
false
```





# 连接合并数组

## arr.concat( arr )

> 目标数组.concat(源数组)

```ruby 
# 把arr_2添加到arr_1中
arr_1.concat(arr_2)
```

```ruby 
a = [1,2,3]
b = [4,5,6]

a.concat(b)
p a    
p b

#终端输出结果：
[1, 2, 3, 4, 5, 6]
[4, 5, 6]
```



## arr + arr   

```ruby 
# 把arr_2和arr_1元素合并，返回一个新数组
arr_new = arr_1 + arr_2
```

```ruby 
a = [1,2,3]
b = [4,5,6]

c= a + b
p c
p a    
p b

#终端输出结果：
[1, 2, 3, 4, 5, 6]
[1, 2, 3]
[4, 5, 6]
```



## *arr

```ruby
# 把数组的元素展开

print *[1,2,3] 
1 2 3
```

```ruby
a = [4,5]
b = [1,2,3,6,7,8]

c = [1,2,3,*a,6,7,8]
p c

#终端输出结果：
[1, 2, 3, 4, 5, 6, 7, 8]
```

```ruby
a = [4,5]

p [1,2,3,a]
p [1,2,3,*a]

#终端输出结果：
[1, 2, 3, [4, 5]]
[1, 2, 3, 4, 5]
```

```ruby
a = [4,5]

p *a
p a
p *a[0]

#终端输出结果：
4
5
[4, 5]
4
```



# 遍历数组

## arr.each do end

```ruby 
arr.each do |自定义名|
    处理程序 
end
# |自定义变量|是数组的元素
```

### 执行相同操作

```ruby 
# 执行的次数是数组元素的个数
arr.each do 
   处理程序 
end
```

```ruby 
a = [1,2,3,4]

a.each do
    p "Heloo Ruby"
end

#终端输出结果：
"Heloo Ruby"
"Heloo Ruby"
"Heloo Ruby"
"Heloo Ruby"
```

### 执行不同操作（使用数组元素）

```ruby 
arr.each do |item|
    处理程序 item 
end
```

```ruby 
# 打印数组的元素
a = [1,2,3,4]

a.each do |v|
    p v
end

#终端输出结果：
1
2
3
4
```

```ruby
# 数组元素求和
# 在遍历外声明sum变量，在循环里给sum赋值，循环结束后输出sum
a = [1,2,3,4]

sum = 0
a.each do |v|
    sum += v
end
p sum

#终端输出结果：
10
```

```ruby
# 例子：删除数组中的奇数元素
arr = [1,2,3,4,5,6]

arr.each do |v|
    if v .odd?
        arr.delete(v)
    end
end
p arr

#终端返回的结果：
[2, 4, 6]


# 或者使用 delete_if do end
arr = [1,2,3,4,5,6]
arr.delete_if do |v|
    v.odd?
end
p arr

#终端返回的结果：
[2, 4, 6]
```



## arr.each { }

```ruby
# 和array.each do 一样
# 利用块级作用域的思想
# 用{ |item| } 代替 do |item| end
# { }可以写在一行，更简便
```

```ruby
arr.each do |item| 
    处理
end

arr.each{ |item| 处理 }
```

```ruby
arr = [1,2,3,4]

sum = 0
arr.each do |v|
    sum += v
end
p sum

#终端返回的结果：
10


#或使用{ }：
arr = [1,2,3,4]

sum = 0
arr.each { |v| sum += v }
p sum

#终端返回的结果：
10
```

### 多维数组的多参数

```ruby
# 可以代入多个参数，但不能超过实际数组中层数
多维数组arr.each{ |item，item,..| 处理 }
```

```ruby
#不能超过实际数组中的元素,找不到的返回nil
arr = [100,200,300]
arr.each{|a,b,c|
	p a
    p b
    p c
}

#终端返回的结果：
100
nil
nil
200
nil
nil
300
nil
nil
```

```ruby
# 多维数组
# 求面积 长*宽，结果放入数组
cubs = [[10,20],[10,30],[10,40]]
area = []

#利用一个参数：
cubs.each{|v| 
	length = v[0]
    width = v[1]
    area << length*width
}
p area

#利用多个参数：
cubs.each{|length,width|
	cubs.each{|length,width|
    area << length*width
}
p area   


#终端返回的结果：
[200, 300, 400]
```





## ?arr.each_with_index{ }

```ruby
# .each{|item|}只能操作元素值，
# .each_with_index{|item,index|}多了可以同时操作元素序号
array.each_with_index{|item,index| 处理}
```

```ruby
# 只能用来代替 .each{}
# 不能和.map()连用
```

```ruby
arr = ["赛文奥特曼","杰克奥特曼","艾斯奥特曼"]
arr.each_with_index{|v,i|
p "#{i}.#{v}"
}

#终端返回的结果：
"0.赛文奥特曼"
"1.杰克奥特曼"
"2.艾斯奥特曼"


arr = ["赛文奥特曼","杰克奥特曼","艾斯奥特曼"]
arr.each_with_index{|v,i|
p "#{i+1}.#{v}"
}

#终端返回的结果：
"1.赛文奥特曼"
"2.杰克奥特曼"
"3.艾斯奥特曼"
```







# 操作数组

## ？arr.inject( ){ }

```ruby

```

```ruby

```

```ruby

#终端返回的结果：
```

## ？.with_index

```ruby
# 可以和其他有变量操作的方法连用
# 比如.map()、delete()
arr.map.with_index{|item,index| 处理}
```

```ruby
arr = [10,20,30]
arr.map.with_index{ |v,i|
    v*10
    p "#{i} : #{v*10}"
}

#终端返回的结果：
"0 : 100"
"1 : 200"
"2 : 300"
"3 : 400"
"4 : 500"
```

```ruby
#用.reject{}
arr = [10,20,30,40,50]
arr.reject.with_index{ |v,i|
    v.odd?
    p "#{i} : #{v*10}"
}

#或用.delete_if{}

#终端返回的结果：
"0 : 100"
"1 : 200"
"2 : 300"
"3 : 400"
"4 : 500"
```

```ruby

```



```ruby

```

```ruby

```

```ruby

```

```ruby

```

# 范围

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

