

# 含义

```ruby
# 类似class类
```

```ruby
module 模块名
end
```

```ruby

```



# 其他文件引入模块

## require_relative "./文件路径 "

```ruby
# 引入装有模块的文件
require "装有要调用模块的文件的绝对地址"
或：
require_relative "装有要调用模块的文件相对于当前文件的相对地址"
```

```ruby

```



# class类引入模块

## require_relative "./文件路径"

```ruby
# 调用模块第一步：
# 引入装有模块的文件
require "装有要调用模块的文件的绝对地址"
或：
require_relative "装有要调用模块的文件相对于当前文件的相对地址"
```

## include 模块名 

```ruby
# 调用模块第二步：
# class类中引入模块

class 类名
    include 模块名
end
```

```ruby
# 装有模块的文件：
module Aa
    def say_hello
        p "我是被调用的模块"
    end
end

# 调用模块的文件：
require_relative "./exports"
class Bb
    include Aa
end

sample = Bb.new
sample.say_hello


# 终端显示结果：
"我是被调用的模块"
```

```ruby

```





# 模块中的变量



---

## 在其他文件中被调用

```ruby
#首先要引入模板所在的文件
#然后 ::变量名 调用

require_relative "模块文件的相对地址"
调用的模块名::变量
```

```ruby
# 模块文件
module M
  Say_hello = "我是模块的变量"
end

# 引入模块的文件
require_relative "./exports"
p M::Say_hello

# 终端显示结果：
"我是模块的变量"
```

---

## 在class类中被调用

### 直接被调用

```ruby

```

```ruby
class 类名
    include 模块名
    模块::变量
end
```

```ruby
# 模块文件
module M
  Price = 100
end

# 引入模块的文件
require_relative "./exports"
class Samples
    include M
    def show
        "#{M::Price} 元"
    end
end
sample = Samples.new
p sample.show
p M::Price

p sample.respond_to?("Price")
p Samples.respond_to?("Price")

# 终端显示结果：
"100 元"
100
false
false
```

---

### 赋值给类的属性

```ruby

```

```ruby
class 类名
    include 模块名
    attr_accessor :属性名
    属性 = 模块的变量名
end
```

```ruby
# 模块文件
module M
  Price = 100
end

# 引入模块的文件
require_relative "./exports"

class Samples
    include M
    attr_accessor :price

    def initialize
        price = Price
    end
    def change
        price = 200
    end
end
sample = Samples.new
p sample.change
p M::Price

p sample.respond_to?("price")
p Samples.respond_to?("price")
p sample.respond_to?("Price")
p Samples.respond_to?("Price")

# 终端显示结果：
200
100

true
false
false
false
```

---

### 赋值给实例化变量@@

```ruby

```

```ruby
class 类名
    include 模块名
    def initialize
        @实例化变量 = 模块变量名
    end
end
```

```ruby
# 模块文件
module M
  Price = 100
end

# 引入模块的文件
require_relative "./exports"
class Samples
    include M
    def initialize
        @price = Price
    end
    def change
        @price = 200
    end
end
sample = Samples.new
p sample.change
p M::Price
p sample.respond_to?("Price")
p Samples.respond_to?("Price")

# 终端显示结果：
200
100
false
false
```

```ruby

```

---

## 在class类中修改

```ruby

```

```ruby
# 模块文件
module M
  Price = 100
end

# 引入模块的文件
require_relative "./exports"

class Samples
    include M
    def message
       M::Price.to_f.to_s
    end
end
sample = Samples.new
p sample.message
p M::Price

# 终端显示结果：
"100.0"
100
```

```ruby

```

```ruby

```

```ruby

```

```ruby

```



# 模块中的方法

## 实例化方法

```ruby
# 实例化方法
# 直接在模块中定义的方法都是实例化方法
```

```ruby
module 模块名
    def 方法名
    end
end
```

### 只能被class实例化对象调用

```ruby
# 不能在模块外被直接调用
# 引用模块class类在实例化后，实例化对象可以使用
```

```ruby
class 类名
    include 模块名
end
实例化对象 = 类名.new
实例化对象.模块的实例化方法
```

```ruby
# 模块文件
module M
    def fn1
        p "我是模块的实例化方法fn1"
    end
    def fn2
        p "我是模块的实例化方法fn2"
    end
end

# 引入模块的文件
require_relative "./exports"
class Samples
    include M
end
sample = Samples.new
sample.fn1
sample.fn2

p sample.respond_to?("fn1")
p sample.respond_to?("fn2")

# 终端显示结果：
"我是模块的实例化方法fn1"
"我是模块的实例化方法fn2"
true
true
```

```ruby

```



## 静态方法

```ruby
# 静态化方法
# 在模块内声明方法后，并用下列代码指明的方法
module_function :方法名
```

```ruby
module 模块名
    def 方法名
    end
    module_function :方法名
end
```

### 可以在其他文件中被调用

```ruby
# 可以直接在模块外被调用静态方法
```

```ruby
# 模块文件
module M
    def fn1
        p "我是模块的静态方法fn1"
    end
    module_function :fn1
end

# 引入模块的文件
require_relative "./exports"
M.fn1

# 终端显示结果：
"我是模块的静态方法fn1"
```

---

### 可以在类中被调用

```ruby
# 因为可以直接在模块外被调用静态方法
# 所以在类中也可以调用模块的静态方法
```

```ruby
class 类名
    include 模块名
    def 类的实例化方法
        模块.静态方法
    end
end
```

```ruby
# 模块文件
module M
    def fn1
        p "我是模块的静态方法fn1"
    end
    module_function :fn1
end

# 引入模块的文件
require_relative "./exports"
class Samples
    include M

    def message
        M.fn1
    end
end
sample = Samples.new
sample.message

p sample.respond_to?("fn1")
p Samples.respond_to?("fn")

# 终端显示结果：
"我是模块的静态方法fn1"
false
false
```

```ruby

```

---

### 不能被class实例化对象调用

```ruby
# 引用模块class类在实例化后，实例化对象不能使用模块的静态方法
```

```ruby
# 引入模块的文件
require_relative "./exports"
class Samples
    include M
end
sample = Samples.new
p sample.respond_to?("fn1")
sample.fn1

# 终端显示结果：
false
c:/Users/USER/Desktop/Ruby/module/require.rb:8:in `<main>': private method `fn1' called for #<Samples:0x00000000050db9c0> (NoMethodError)
```



## self.方法

```ruby
# 是模块自己的方法，只能被自己调用
```

```ruby
module 模块名
    def self.方法名
    end
end
```

### 只能被模块自己调用

```ruby
module 模块名
    def self.方法名
    end
end
模块名::方法名
模块名.方法名
```

```ruby
# 模块文件
module M
    def self.fn1
        p "我是模块的self方法fn1"
    end
end

# 引入模块的文件
require_relative "./exports"
class Samples
    include M
end
sample = Samples.new
p sample.respond_to?("fn1")
p Samples.respond_to?("fn1")

M::fn1
M.fn1

# 终端显示结果：
false
false
"我是模块的self方法fn1"
"我是模块的self方法fn1"
```

---

### 可以在类中被调用

```ruby
class 类名
    include 模块名
    def 类的实例化方法
        模块::方法名
    end
end
```

```ruby
# 模块文件
module M
    def self.fn1
        p "我是模块的self方法fn1"
    end
end

# 引入模块的文件
require_relative "./exports"
class Samples
    include M
    def message
        M::fn1
    end
end
sample = Samples.new
sample.message

p sample.respond_to?("fn1")
p Samples.respond_to?("fn1")

# 终端显示结果：
"我是模块的self方法fn1"
false
false
```

# 





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

