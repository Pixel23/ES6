### 第一节let和const命令

##### 一. let命令

1. let用于声明变量，类似var,所声明的变量只在let命令所在代码块有效
2. 不存在变量提升，所声明的变量一定要在声明后使用，否则报错
3. 暂时性死区:只要块级作用域内存在let命令，他所声明的变量就“绑定”这个区域，不再受外界影响
       var tmp = 123;
       if (true) {
         tmp = 'abc'; // ReferenceError
         let tmp;
4. 不允许重复声明

##### 二.es6的块级作用域，块级作用域与函数的声明

1. 明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。 

##### 三.const命令

1. 声明一个常量，一旦声明，常量的值就不能改变
2. 只在声明所在的块级作用域内有效 
3. const声明变量不可提升，存在暂时性死区，不可重复声明

##### 四.本质

	const声明的变量，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指针，const只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。 

    const foo = {};
    
    // 为 foo 添加一个属性，可以成功
    foo.prop = 123;
    foo.prop // 123
    
    // 将 foo 指向另一个对象，就会报错
    foo = {}; // TypeError: "foo" is read-only

上面代码中，常量foo储存的是一个地址，这个地址指向一个对象。不可变的只是这个地址，即不能把foo指向另一个地址，但对象本身是可变的，所以依然可以为其添加新属性。 

ES6 声明变量的六种方法:ES5 只有两种声明变量的方法：var命令和function命令。ES6 除了添加let和const命令，后面章节还会提到，另外两种声明变量的方法：import命令和class命令。所以，ES6 一共有 6 种声明变量的方法。



### 第二节 变量的解构赋值

##### 一.数组的解构赋值

1. ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。 
       let [foo, [[bar], baz]] = [1, [[2], 3]];
       foo // 1
       bar // 2
       baz // 3
       
       let [ , , third] = ["foo", "bar", "baz"];
       third // "baz"
       
       let [x, , y] = [1, 2, 3];
       x // 1
       y // 3

##### 二.函数解构

1. 函数参数的解构也可以使用默认值
       function move({x = 0, y = 0} = {}) {
         return [x, y];
       }
       
       move({x: 3, y: 8}); // [3, 8]
       move({x: 3}); // [3, 0]
       move({}); // [0, 0]
       move(); // [0, 0]
   x,y默认值为0，0

##### 第三节 函数扩展

1.设置参数默认值

    function log(x, y = 'World') {
      console.log(x, y);
    }
    
    log('Hello') // Hello World
    log('Hello', 'China') // Hello China
    log('Hello', '') // Hello
    

2.函数length属性

指定默认值之后，函数的length属性，将返回没有制定默认值的参数个数，制定默认值后，length将失真

3.作用域

一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域（context）。等到初始化结束，这个作用域就会消失。这种语法行为，在不设置参数默认值时，是不会出现的。

    var x = 1;
    
    function f(x, y = x) {
      console.log(y);
    }
    
    f(2) // 2

上面代码中，参数y的默认值等于变量x。调用函数f时，参数形成一个单独的作用域。在这个作用域里面，默认值变量x指向第一个参数x，而不是全局变量x，所以输出是2。

    let x = 1;
    
    function f(y = x) {
      let x = 2;
      console.log(y);
    }
    
    f() // 1

上面代码中，函数f调用时，参数y = x形成一个单独的作用域。这个作用域里面，变量x本身没有定义，所以指向外层的全局变量x。函数调用时，函数体内部的局部变量x影响不到默认值变量x。

如果此时，全局变量x不存在，就会报错。

    function f(y = x) {
      let x = 2;
      console.log(y);
    }
    
    f() // ReferenceError: x is not definedres't

4.rest参数

用于获取函数的多余参数，这样就不需要使用arguments对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中

    function add(...values) {
      let sum = 0;
    
      for (var val of values) {
        sum += val;
      }
    
      return sum;
    }
    
    add(2, 5, 3) // 10

5.箭头函数

    var  sum = (num1, num2) => { return num1 + num2};

# 注意事项

（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

    function foo() {
      setTimeout(() => {
        console.log('id:', this.id);
      }, 100);
    }
    
    var id = 21;
    
    foo.call({ id: 42 });

（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

（4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。




