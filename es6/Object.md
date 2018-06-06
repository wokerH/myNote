# ES6 中的Object

### 属性和方法的简写

支持直接写入变量和函数，作为对象的属性和方法

##### 属性简写：

```javascript
const foo = 'bar';
const baz = {foo};
console.log(baz); // {foo: 'bar}
```

##### 方法简写：

```javascript
const obj = {
    method() {
        return 'this is a method!'
    }
}
```

##### 注意：简写的属性名总是字符串  

### 属性名表达式

##### JavaScript中如何定义对象的属性

```javascript
// 方式一
obj.property = 'aaa';

// 方式二
obj['property'] = 'aaa';
```

##### JavaScript中使用字面量定义对象

```javascript
var obj = {
    p1: 'aaa',
    p2: 'bbb'
};
```

在ES5中使用上面字面量方式定义对象时，其中的属性是没办法使用表达式的，而在ES6中，可以将属性名放在方括号内使用相关表达式，如下：

```javascript
let pKey = 'p1';

let obj = {
    [pKey]: 'aaa',
    ['p' + '2']: 'bbb'
};
```

同样的，ES6中表达式可用于定义方法名：如下

```javascript
let obj = {
    ['m' + 'Say']() {
        return 'hello';
    }
}
```

##### 注意：

- 属性名表达式和简写方式不能同时使用；
- 属性名表达式如果是一个对象，系统会自动调用toString()方法将对象转为对象的字符串形式



### 方法的name属性

##### 一般方法的name属性

一般的，方法的neme属性是指其方法名。

```javascript
const animal = {
    eat() {
        return 'eatting...'
    },
}
console.log(animal.eat.name); // "eat"
```

##### 对象setter和getter的name属性

如果对象的方法使用了去值函数 **getter** 和存值函数**setter**，那么此时的name属性不是在该调用方法上，而是在该方法的属性的**描述对象**上的get和set属性上，返回值是方法名前加上get和set。

```javascript
const obj = {
    get func() {
        // to do something...
    },
    set func(x) {
        // to do something...
    }
};
obj.func.name; // TypeError: Cannot read property 'name' of undefined
const objDsc = Object.getOwnPropertyDescriptor(obj, 'func'); // 获取func方法的属性描述对象

objDsc.get.name; // "get func"
objDsc.set.name; // "set func"
```

##### bind方法创造的函数的name属性

通常bind方法创造的函数，其name属性之为 ```"bound"```加上原函数名。

```javascript
var myFunc = function() {
	// do something...
};
myFunc.bind().name; // "bound myFunc"
```

##### Function构造函数创造的函数

由Function构造函数创造的函数的name属性为```anonymous```(匿名)。

##### 方法名为Symbol类型的方法

其name属性为该Symbol值的描述。



### Object.is()

##### ES5中判断两个值相等的方式

- 相对运算符  ```==```
- 全等运算符 ```===```

以上两种方式在大多数情况下使用都是很方便的，但是他们在某些情况下存在一下缺陷：

- 相等运算符会自动进行类型转换

- 全等运算符中 ```NaN``` 不等于自身

- ```+0``` 等于 ```-0```

- 等待...

  

##### ES6中的同值相等算法

在ES5中，缺乏一种运算，在所有环境中，只要两个值是一样的，它们就应该相等。而ES6的同值相等算法就是为了解决这一问题而提出的。其实现方法就是 ```Object.is()``` ,其行为与严格相等运算符基本一致， 不同点主要有两点：

-  ```+0``` 不等于 ```-0```
- ```NaN``` 等于自身

```javascript
console.log( +0 === -0 ); // true
console.log( NaN === NaN ); // false

console.log( Object.is(+0, -0) ); // false
console.log( Object.is(NaN, NaN) ); // true
```



### Object.assign()

##### 基本用法

###### 作用：

​	用于对象的合并，即：将源对象（source）的所有可**枚举**属性复制到目标对象（target）。

###### 参数：

​	第一个参数是目标对象，后面的参数都是源对象。

###### 返回值：

​	复制好源对象可枚举属性后的目标对象。

###### 参数变化：

-  源对象不变
- 目标对象改变

```javascript
const target = { a: 1, b: 1 };

const source1 = { b: 2, c: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2); // {a: 1, b: 2, c: 3}
```



###### 注意：

- 如果目标对象和源对象有重名的属性，或者多个源对象有重名的属性，则后面的属性会覆盖前面的属性。
- 如果只有一个参数，则直接返回该参数； 如果该参数不是对象，则会先转成对象，再返回；由于undefined和null无法转成对象，所有他们作为参数时会报错。
- 如果非对象参数出现在源对象的位置（即非首参数） ；首先，这些参数都会转成对象，如果无法转成对象，就会跳过。这意味着，如果`undefined`和`null`不在首参数，就不会报错；但是，除了字符串会以数组形式，拷贝入目标对象，其他值都不会产生效果。 
- `Object.assign`拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（`enumerable: false`）。 
- 属性名为 Symbol 值的属性，也会被`Object.assign`拷贝 。



##### 注意事项

###### 浅拷贝：

`Object.assign`方法实行的是浅拷贝，而不是深拷贝；即：如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。

```javascript
const obj1 = {
    a: {
        a1: 1
    }
};
const obj2 = Object.assign({}, obj1);
obj1.a.a1 = 2;
obj2.a.a1; // 2
```

###### 同名属性替换：

```javascript
const target = {a: {a1: 'aaa', a2: 'bbb'}}
const source = {a: {a1: 'ccc'}}
Object.assign(target, source) // {a: {a: 'ccc'}}
```

对于上面这种嵌套的对象，一旦遇到同名属性，`Object.assign`的处理方法是替换，而不是添加 。

###### 数组的处理：

`Object.assign`可以用来处理数组，但是会把数组视为对象，其属性名为数组的索引。如：

```javascript
Object.assign([1,2,3],[4,5]); // [4,5,3]
```

###### 取值函数的处理：

`Object.assign`只能进行值的复制，如果要复制的值是一个取值函数，那么将求值后再复制 。

```javascript
const source = {
    get func() {
        return 1;
    }
};
const target = {};
Object.assign(target, source); // {func: 1}
```



##### 开发用途

###### 给对象添加属性：

```javascript
class Point {
    constructor(x, y) {
        Object.assign(this, {x, y});
    }
}
```

###### 给对象添加方法：

```javascript
Object.assign(SomeClass.prototype, {
    methods1(arg1, arg2) {
        // ...
    },
    methods2() {
        // ...
    }
});
```

###### 克隆对象：

```javascript
// 该方法只能拷贝原始对象自身，不能克隆它继承的值
function clone(origin) {
    return Object.assign({}, origin);
}

// 可以克隆继承链的方法
function cloneProto(origin) {
    let originProto = Object.getPrototypeOf(origin);
    return Object.assign(Object.create(originProto), origin);
}
```

###### 合并多个对象：

```javascript
const merge = (target, ...source) => Object.assign({}, ...source);
```

###### 为属性指定默认值：

```javascript
const DEFAULTS = {
    logLevel: 0,
    outputFormat: 'html'
};

function processContent(options) {
    options = Object.assign({}, DEFAULTS, options);
    // TODO ...
}
```



### 属性的可枚举性和遍历

##### 可枚举性

对象的每个属性都有一个描述对象（Descriptor），用来控制该属性的行为，一般是通过 `Object.getOwnPropertyDescriptor()` 方法来获取该属性的描述对象。

```javascript
let obj = { a: 'aaa'};
Object.getOwnPropertyDsecriptor(obj, 'a');
// {
// 	   value: 'aaa',
//     writable: true,
//     enumerable: true,
//     configurable: true
// }
```

其中，`enumerable` 属性为可枚举性属性，`true` 表示可枚举，如果为`false` 表示为不可枚举。目前有四个操作会忽略 `enumerable`为`false`的属性：

- `for...in` 循环：只遍历对象**自身**的属性和**继承**的可枚举的属性。
- `Object.keys()`：返回对象**自身**的所有可枚举的属性的键名。
- `JSON.stringify()`：只串化对象**自身**的可枚举的属性。
- `Object.assign()`：只拷贝对象**自身**可枚举的属性。

另外，ES6 规定，所有 Class 的原型的方法都是不可枚举的 

##### 属性的遍历

###### `for...in`：

循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。 

###### `Object.keys(obj)`：

返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名 。

###### `Object.getOwnPropertyNames(obj)`：

返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。 

###### `Object.getOwnPropertySymbols(obj)`：

返回一个数组，包含对象自身的所有 Symbol 属性的键名。 

###### `Reflect.ownKeys(obj)`：

返回一个数组，包含对象自身的所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。 

###### 说明：

以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则：

- 首先遍历所有数值键，按照数值升序排列。
- 其次遍历所有字符串键，按照加入时间升序排列。
- 最后遍历所有 Symbol 键，按照加入时间升序排列。



### Object.getOwnPropertyDescriptors()

##### 核心用途

该方法是ES2017新引入的方法，用于返回指定对象所有的自身属性的描述对象。

该方法的引入目的，主要是为了解决`Object.assign()`无法正确拷贝`get`属性和`set`属性的问题 。

```javascript
const source = {
    set func(value) {
    	// ...
    }
};
const target1 = {};
Object.assign(target1, source);  // {func: undefined}

Object.getOwnPropertyDescriptor(target1, 'func')
// { value: undefined,
//   writable: true,
//   enumerable: true,
//   configurable: true }
```

上面代码中，`source`对象的`func`属性的值是一个赋值函数，`Object.assign`方法将这个属性拷贝给`target1`对象，结果该属性的值变成了`undefined`。这是因为`Object.assign`方法总是拷贝一个属性的值，而不会拷贝它背后的赋值方法或取值方法。 

这是，如果用`Object.getOwnPropertyDescriptors`方法配合`Object.defineProperties`方法，就可以实现正确拷贝。

```javascript
const source = {
    set func(value) {
        // ...
    }
};
const target2 = {};
Object.defineProperties(target2, Object.getOwnPropertyDescriptors(source));
Object.getOwnPropertyDescriptor(target2, 'func')
```

 ##### 其他用途（了解）

- 配合`Object.create`方法，将对象属性克隆到一个新对象。这属于浅拷贝。 
- 实现一个对象继承另一个对象 。
- 来实现 Mixin（混入）模式。 



### 原型链操作

##### `__proto__`属性

用来读取或设置当前对象的`prototype`对象。目前，所有浏览器（包括 IE11）都部署了这个属性 。ES6和ES5有不同的写法。

```javascript
// ES5
var obj = {
    method: function() {
        // ...
    }
};
obj.__proto__ = otherObj;

// ES6
var obj = Object.create(otherObj);
obj.method  function() { ... }
```

注意： 标准明确规定，只有浏览器必须部署这个属性，其他运行环境不一定需要部署，而且新的代码最好认为这个属性是不存在的。因此，无论从语义的角度，还是从兼容性的角度，都不要使用这个属性，而是使用下面的`Object.setPrototypeOf()`（写操作）、`Object.getPrototypeOf()`（读操作）、`Object.create()`（生成操作）代替。 

##### Object.setPrototypeOf()

###### 作用：

​	作用与`__proto__`相同，用来设置一个对象的`prototype`对象，返回参数对象本身。它是 ES6 正式推荐的设置原型对象的方法 

###### 参数：

- 第一个参数： 需要设置原型的对象
- 第二个参数： 原型对象

###### 返回值：

​	参数对象本身

```javascript
let proto = {};
let obj = {x: '123'};
Object.setPrototypeOf(obj, proto);

proto.y = '345';
proto.z = '678';

obj.x; // "123"
obj.y; // "345"
obj.z; // "678"
```

###### 注意：

- 如果第一个参数不是对象，会自动转为对象。但是由于返回的还是第一个参数，所以这个操作不会产生任何效果。 
- 由于`undefined`和`null`无法转为对象，所以如果第一个参数是`undefined`或`null`，就会报错 。

##### Object.getPrototypeOf()

###### 作用：

- 读取一个对象的原型对象

###### 参数：

- 需要获取其原型对象的对象

###### 返回值：

- 对象的原型对象

```javascript
function Animal() {
    // ...
}

const dog = new Animal();
Object.getPrototypeOf(dog) === Animal.prototype; // true

Object.setPrototypeOf(dog, Object.prototype);
Object.getPrototypeOf(dog) === Animal.prototype; // false
```

###### 注意：

- 如果参数不是对象，会被自动转为对象。
- 如果参数是`undefined`或`null`，它们无法转为对象，所以会报错。  



### super关键字

在ES6中新增了 `super` 关键字，其指向的是当前对象的原型对象。

注意，`super`关键字表示原型对象时，只能用在对象的**方法**之中，用在其他地方都会报错。如下： 

```javascript
const obj = {
    foo: super.foo // 报错，因为super不能用在属性里面
}

// 一下两个均报错
// 因为目前，只有对象方法的 简写 法可以让 JavaScript 引擎确认，定义的是对象的方法。
const obj = {
    foo: () => super.foo
}
const obj = {
    foo: function () {
        return super.foo
    }
}
```



### Object.keys()，Object.values()，Object.entries() 的比较

三个方法都是遍历对象的自身可枚举的属性

- `Object.keys()` : 返回键名组成的数组

- `Object.values()` : 返回属性值组成的数组

- `Object.entries()` :   返回键值对的二维数组

  

### 扩展运算符 `...`

ES2018将这个运算符引入了对象。

##### 解构赋值

###### 基本用法：

对象的解构赋值用于从一个对象取值，相当于将目标对象自身的所有可遍历的（enumerable）、但尚未被读取的属性，分配到指定的对象上面。**所有的键和它们的值，都会拷贝到新对象上面**。 

```javascript
let {x, y, ...z} = {x:1, y:2, a:3, b:4};
x // 1
y // 2
z // {a: 3, b: 4}
```

###### 注意：

- 由于解构赋值要求等号右边是一个对象，所以如果等号右边是`undefined`或`null`，就会报错，因为它们无法转为对象 
- 解构赋值必须是最后一个参数，否则会报错。 

##### 扩展运算符

对象的扩展运算符（`...`）用于取出参数对象的所有可遍历属性，**拷贝**到当前对象之中，等同于使用`Object.assign`方法  。

```javascript
let z = { a: 3, b: 4 };
let n = { ...z };
n // { a: 3, b: 4 }
```

