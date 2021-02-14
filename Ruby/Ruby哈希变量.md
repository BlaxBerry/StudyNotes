# 含义

```ruby
# 一组键值对 {key:value}就是哈希
{key => value}
```

```ruby
# 类似数组，但是数组是元素是有序号顺序的
# Hash对象没有顺序，只有键值一一对应
```

```ruby


```





# 创建Hash

## { }

### {'key'=>value}

```ruby
# 字符串的key => value
hash = {key => value,key => value,...}
```

```ruby
hash = {
    "name" => "Andy",
    "age" => 25
}
p hash

# 终端输出结果：
{"name"=>"Andy", "age"=>25}
```



### {key:value}

```ruby
# key是个symbol
# 推荐
```

```ruby
hash = {
    a:10,
    b:20,
    c:30
    }
p hash #=> 

{:a=>10,:b=>20,:c=>30}
```

```ruby
student = {
    name:"Andy",
    age:28,
    score:[80,80,90]
    } 
p student

# 终端输出结果：
{:name=>"Andy", :age=>28, :score=>[80, 80, 90]}
```



## Hash.new( )

```ruby
# 创建一个空Hash
hsh = Hash.new
```

```ruby
hash = Hash.new
p hash
p hash[1]
p hash["a"]
p hash

# 终端输出结果：
{}
nil
nil
{}
```

```ruby

```

```ruby

```

```ruby
hash = Hash.new{|hash,key|
    hash[key] = "I am key"
    }

```

```ruby

```





# 键key

```ruby
#Hash中的键必须是唯一的，不能重复。
# 因此可以使用symbol
```

```ruby

```

## hash.keys

```ruby
# 获得哈希中所有的键，
# 以数组方式返回
```

```ruby
hash = {
    "name" => "Andy",
    "age" => "25",
    "sex" => "male"
}
p hash.keys

# 终端输出结果：
["name", "age", "sex"]
```



## hash.has_key?( )

```ruby
# 判断哈希对象中是否有某个键
# 返回true/false
hash.has_key?(:key)
或
has.has_key?("key")
```

```ruby
hash = {"name"=>"Andy","age"=>25}
p hash.has_key?("name")
p hash.has_key?("score")

# 终端输出结果：
true
false
```

```ruby
hash = {name:"Andy",age:25}
p hash.has_key?(:name)
p hash.has_key?(:score)

# 终端输出结果：
true
false
```

```ruby

```

```ruby

```



# 值value

```ruby
# hash的value可以是任何数据
# 可以是数字、字符串、数组、函数、也可以是一个hash
```

```ruby
hash = {
    "num" => 100,
    "str" => "hello",
    "arr" => [1,2,3],
    "fn" => def fn 
    end,
    "ha" => {
        "a" => 1,
        "b" => 2
    }
}
p hash

# 终端输出结果：
{"num"=>100, "str"=>"hello", "arr"=>[1, 2, 3], "fn"=>:fn, "ha"=>{"a"=>1, "b"=>2}}
```



## hash.values

```ruby
# 获得哈希对象中的所有的值
```

```ruby
hash = {
    "name" => "Andy",
    "age" => "25",
    "sex" => "male"
}
p hash.values

# 终端输出结果：
["Andy", "25", "male"]
```

```ruby

```

```ruby

```



# 获取值

## hash[‘key’]

```ruby
# 用于 字符串 => value 的形式创建的哈希
```

```ruby
hash = {'key'=>value,'key'=>value,...}
hash['key']
```

```ruby
student= {
    "name" => "Andy",
    "age" => 28,
    "point" => [80,75,90],
    "score" =>{
        "english" => 80,
        "math" => 75,
        "history"=>90
    }
}
p student["name"]
p student["age"]
p student["point"][0]
p student["score"]["english"]

# 终端输出结果：
"Andy"
28
80
80
```

```ruby

```



## hash[:key]

```ruby
# 用于获取{key:value}创建的哈希
```

```ruby
hash = {
    key1:value,
    key2:value,
    ...
    }

hash[:key1]
hash[:key2]
```

```ruby
student= {
    name:"Andy",
    age:28,
    point:[80,75,90],
    score:{
        english:80,
        math:75,
        history:90
    }
}
p student[:name]
p student[:age]
p student[:point][0]
p student[:score][:english]

# 终端输出结果：
"Andy"
28
80
80
```

```ruby

```



# 默认值

## hash.default

```ruby

```

```ruby

```

```ruby

```





# 判断哈希对象是否为空

## hash.empty?

```ruby
# 判断是否是个空
```

```ruby
# 返回值是true / false
```

```ruby
hash = {}
p hash.empty?

hsh = {name:"andy",age:25}
p hsh.empty?

# 终端输出结果：
true
false
```

```ruby

```



# 判断哈希对象等于

## hash.eql?( )

```ruby
# 判断一个哈希是否等于另一个哈希

hash1.eql(hash2)
```

```ruby
hash = {}
hsh = {name:"andy",age:25}

p hash.eql?(hsh)
p hash.eql?({})

# 终端输出结果：
false
true
```

```ruby

```



# 长度

## hash.length/hash.size

```ruby
# 就是返回hash对象中有几个键值对
```

```ruby
hash = {}
hsh = {name:"andy",age:25}

p hash.length
p hsh.length

# 终端输出结果：
0
2
```

```ruby

```

```ruby

```



# 删除键值对

## hash.delete( "key")

```ruby

```

```ruby

```

```ruby

```

```ruby

```



## hash.delete( :key)

```ruby

```

```ruby

```

```ruby

```

```ruby

```



## hash.delete_if{ }

```ruby
# 遍历循环哈希，删除指定key或value的键值对
# 如果条件表达式值为true则删除
 
```

```ruby
hash.delete_if{|key,value|
    筛选条件
    }
```

```ruby

```



## hash.keep_if{ }

```ruby
# 遍历循环哈希，保留满足条件的键值对
# 删除不满足条件的键值对
# 判断条件值为true 则保留
```

```ruby
hash.keep_if{|key,value|
    筛选条件
    }
```

```ruby

```



## hash.clear

```ruby
# 清空一个哈希变量中所有键值对
# 慎用
```

```ruby
hsh = {name:"andy",age:25}
p hsh
hsh.clear
p hsh

# 终端输出结果：
{:name=>"andy", :age=>25}
{}
```



## hash.shift

```ruby
# 删除第一个键值对
# 返回值是删除的键值对组成的数组
```

```ruby
hash = {
    name:"Andy",
    age:25,
    sex:"male",
}
hash.shift
p hash

# 返回结果：
{:age=>25, :sex=>"male"}
```

```ruby
hash = {
    name:"Andy",
    age:25,
    sex:"male",
}
p hash.shift

# 返回结果
[:name, "Andy"]
```

```ruby

```

```ruby

```











# 修改键值对

## hash["key"]=value

```ruby

```

```ruby

```

---

## hash[:key]=value

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

```ruby

```

```ruby

```

```ruby

```



## hash.invert

```ruby
# 调换键值对的键和值
# 返回一个新哈希，不影响原来的哈希
```

```ruby

```

```ruby
hash = {
    a:100,
    b:200
}
p hash.invert
p hash

# 终端输出结果：
{100=>:a, 200=>:b}
{:a=>100, :b=>200}
```

```ruby

```





# 转换为数组

## hash.assoc( )

```ruby
#  把一组键值对转换为数组

hash.assoc("key")
或
hash.assoc(:key)
```

```ruby
hash = {
    "name" => "Andy",
    "age" => "25",
    "sex" => "male"
}
hash.assoc("name")
p hash.assoc("name")
p hash

# 终端输出结果：
["name", "Andy"]
{"name"=>"Andy", "age"=>"25", "sex"=>"male"}
```

```ruby
hash = {
    name:"Andy",
    age:25,
    sex:"male",
    score:{
        english:80,
        math:75,
        history:90
    }
}

hash.assoc(:name)
p hash.assoc(:name)
p hash

hash.assoc(:score)
p hash.assoc(:score)

# 终端输出结果：
[:name, "Andy"]
{:name=>"Andy", :age=>25, :sex=>"male", :score=>{:english=>80, :math=>75, :history=>90}}

[:score, {:english=>80, :math=>75, :history=>90}]
```



## hash.flatten

```ruby
# 返回值是将整个哈希转为数组
# 不会改变原来的哈希对象
# 如果值是个哈希，转成数组后还是一个对象
# 如下的score
```

```ruby
{:key1=>value1,:key2=>value2}.flatten
[:key1,value1,:key2,value2]
```

```ruby
student= {
    name:"Andy",
    age:28,
    point:[80,75,90],
    score:{
        english:80,
        math:75,
        history:90
    }
}
p student.flatten
p student

# 终端输出结果：
[:name, "Andy", :age, 28, :point, [80, 75, 90], :score, {:english=>80, :math=>75, :history=>90}]

{:name=>"Andy", :age=>28, :point=>[80, 75, 90], :score=>{:english=>80, :math=>75, :history=>90}}
```



## hash.to_a

```ruby
# 把哈希转为一个数组，不影响原来哈希对象
# 和flatten类似， 不同在于：
# 如果某键值对的值是个哈希，转成数组后该键值对是一个数组
# 如下的score
```

```ruby
hash = {:key=>value,:key=>value}
arr = hash.to_a
p arr
[:key,value,:key,value]
```

```ruby
student= {
    name:"Andy",
    age:28,
    point:[80,75,90],
    score:{
        english:80,
        math:75,
        history:90
    }
}
p student.to_a
p student

# 终端输出结果：
[[:name, "Andy"], [:age, 28], [:point, [80, 75, 90]], [:score, {:english=>80, :math=>75, :history=>90}]]

{:name=>"Andy", :age=>28, :point=>[80, 75, 90], :score=>{:english=>80, :math=>75, :history=>90}}
```

```ruby

```

```ruby

```

```ruby

```





# 遍历哈希

## hash.each { }

```ruby
# 遍历所有的键和值
```

```ruby
hash.each{|key,value|
    #可分别处理键和值
    }
```

```ruby

```



## hash.each_pair { }

```ruby
# 遍历所有的 键值对
```

```ruby
hash.each_pair{|key,value|
    #可分别处理键和值
    }
```

```ruby

```



## hash.each_key { }

```ruby
# 遍历所有的键
```

```ruby
hash.each_value{|key|
    #处理
    }
```

```ruby

```



## hash.each_value { }

```ruby
# 遍历所有的值
```

```ruby
hash.each_value{|value|
    #处理
    }
```

```ruby

```



## hash.select{ }

```ruby
# 获取满足条件的键值对
# 并存入一个新的哈希对象中
```

```ruby

```

```ruby

```

```ruby

```



## hash.reject{ }

```ruby
# 获取所有不满足条件的键值对
# 并存入一个新的哈希对象中
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

```ruby

```

```ruby

```

```ruby

```

