## 定义类，创建实例对象

```js
class Person {
    
}
const a = new Person()
const a = new Person()
```



## 实例对象传参，构造函数接受参数

构造器函数中的 **this** 指向实例对象

```js
class Person {
  constructor(name,age){
    this.name = name;
    this.age = age
  }
}
const a = new Person('andy'，28)
const a = new Person('tom'，18)
```





## 构造器函数，一般函数

- 构造器并不是必须要写的，

  只有对实例进行初始化操作（添加指定属性）时才需要

  构造函数的调用者是创建的该类的实例对象

  所以，构造函数中的 **this**指向调用该函数方法的实例对象

- 一般函数即方法，都被放在了类的**prototype**原型对象上，

  给类的实例对象使用，

  所以，一般函数中的 **this**指向该函数方法的调用者

```js
class Person {
  constructor(name,age){
    this.name = name;
    this.age = age
    console.log(this) // 实例对象example
  }
  
  say(){
    console.log(this) // 调用者
    console.log(this.name,this.age)
  }
}

const example = new Person('Andy',18)
example.say() // 实例调用方法say

const fun = example.say
fun() // 
// Cannot read property 'name' of undefined
```

类中的方法默认开启了严格模式

（直接调用的函数中的this从指向window变为指向undefined）

所以最终this指向undefined



## 继承父类的构造器函数和一般方法

```js
class Person {
  
}

class Student extends Person{
  
}
```

子类继承了父类后，

父类的构造器函数和一般方法可以直接在子类使用

```js
class Person {
  constructor(name,age){
    this.name = name;
    this.age = age
  }
  
  say(){
    console.log(this.name,this.age)
  }
}

class Student {
  
}

const a = new Student('tom',18)
```



## 子类自己特有的属性

若子类除了父类的方法和属性还想有自己特有的

即必须要写自己的构造器函数

```js
class Person {
  constructor(name,age){
    this.name = name;
    this.age = age
  }
  say(){
    console.log(this.name,this.age)
  }
}

class Student {
  constructor(name,age,grade){
    this.name = name;
    this.age = age;
    this.grade = grade;
  }
}

const a = new Student('tom',18,"高一")
```

子类在写构造函数时，除了写自己特有的，

还要重复写和继承使用的父类的构造函数内容，太麻烦

所以需要使用 **super()**，让子类可以直接调用父类的构造函数

通过 **super()** 把子类构造函数接受的参数，传递给和父类共有的属性

 若使用**super()** ，必须放在构造函数中所有内容最前面

```js
class Person {
  constructor(name,age){
    this.name = name;
    this.age = age
  }
  say(){
    console.log(this.name,this.age)
  }
}

class Student {
  constructor(name,age,grade){
    super(name,age)
    this.grade = grade;
  }
}

const a = new Student('tom',18,"高一")
```

综上

如果子类继承父类的构造函数、

且子类除了继承父类构造函数内容还有自己特有的内容

则子类的构造函数中必须使用**super()**，去传递子类接受的参数给子类和父类共同的内容





## 子类重写从父类继承的方法

子类中和父类同名方法时，优先使用子类自己的

```js
class Person {
  constructor(name,age){
    this.name = name;
    this.age = age
  }
  say(){
    console.log(this.name,this.age)
  }
}

class Student {
  constructor(name,age,grade){
    super(name,age)
    this.grade = grade;
  }
  say(){
    console.log(this.name,this.age,this.grade)
  }
}

const a = new Student('tom',18,"高一")
a.say()
```





## 类中的方法的指向步骤

先找到该实例对象的类，——>

再从类的原型链上找到该方法——>

如果该方法时来自继承的父类，则在到父类的原型链上找该方法——>

找到后执行

```js
class Person {
  constructor(name,age){
    this.name = name;
    this.age = age
  }
  say(){
    console.log(this.name,this.age)
  }
}

class Student {
  constructor(name,age,grade){
    super(name,age)
    this.grade = grade;
  }
  say(){
    console.log(this.name,this.age,this.grade)
  }
  study(){
    this
  }
}

const a = new Student('tom',18,"高一")
a.say()
```





直接给类定义属性

```js
class Person {
    gender = "male"
    say = ()=>{
        console.log('hello');
    }
}
const p = new Person()

console.log(p) // {gender: "male", say: ƒ}
console.log(Person); 
console.log(p.gender,p.say) // male ()=>{console.log('hello')}
p.say() // hello
console.log(Person.gender,Person.say); // undefined undefined
```