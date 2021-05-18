# TypeScript

​		始于javaScript,归于javaScript，属于javaScript的超集，它内部包含了javaScript的所有语法，在此基础上还扩展了其他语法。

​			1、预先思考代码逻辑，**预设变量类型和传参类型**，代码更加规范；
​			2、**类型检查校验**，报错提示；
​			3、方便后期维护以及团队其他成员接手开发。

#### 一、变量类型

##### 	1. number类型(包括NaN和infinite)

​		NaN不等于自身，一般和`isNaN()`进行一些类型判断

##### 	2. string类型

##### 	3. undefined 未定义

##### 	4. null  空对象

##### 	5. boolean类型

##### 	6. symbol类型（es6新添加）

​		唯一标识符,`for`,`keyFor`，和Symbols。

##### 	5.数组与元组

​		元组类似Array但可包含不同数据类型。

​		数组的API

1. **every()** 检测数值元素的每个元素是否都符合条件
2. **some()** 检测数组元素中是否有元素符合指定条件
3. **concat()** 连接两个或更多的数组，并返回结果
4. **filter()** 检测数值元素，并返回符合条件所有元素的数组
5. **forEach()** 数组每个元素都执行一次回调函数
6. **indexOf()** 搜索数组中的元素，并返回它所在的位置
7. **join()** 把数组的所有元素放入一个字符串
8. **lastIndexOf()** 返回一个指定的字符串值最后出现的位置，在一个字符串中的指定位置从后向前搜索
9. **map()** 通过指定函数处理数组的每个元素，并返回处理后的数组
10. **pop()** 删除数组的最后一个元素并返回删除的元素
11. **push()** 向数组的末尾添加一个或更多元素，并返回新的长度
12. **reduce()** 将数组元素计算为一个值（从左到右）
13. **reduceRight()** 将数组元素计算为一个值（从右到左）
14. **reverse()** 反转数组的元素顺序
15. **shift()** 删除并返回数组的第一个元素
16. **slice()** 选取数组的的一部分，并返回一个新数组   `slice(start,end)`
17. **sort()** 对数组的元素进行排序
18. **splice()** 从数组中添加或删除元素     ` arrayObject.splice(index,howmany,item1,.....,itemX)`
19. **toString()** 把数组转换为字符串，并返回结果
20. **unshift()** 向数组的开头添加一个或更多元素，并返回新的长度

##### 	6.引用、对象类型，日期，正则表达式

包含Set(不可重复)，Array，Map(键值),WeakMap和WeakSet(弱引用，不会阻止垃圾，且键只能是对象)

##### 	7.枚举

​		不初始化值默认为0，且会依次递增。当所有枚举成员都拥有字面量枚举值时，枚举成员成为了类型。	

```typescript
enum ShapeKind {
    Circle,
    Square,
}

interface Circle {
    kind: ShapeKind.Circle;
    radius: number;
}

interface Square {
    kind: ShapeKind.Square;
    sideLength: number;
}

let c: Circle = {
    kind: ShapeKind.Square,
    //    ~~~~~~~~~~~~~~~~ Error!
    radius: 100,
}
```

##### 	8. void类型

​		一般用于方法，表示不返回值。

##### 	9. any类型

​		表示可以是任何数据类型。

##### 	10. never类型

​		表示无法达到终点的函数等从来没有返回值的函数

##### 	11.联合类型

​		表示属性可以是几种类型之一，`变量名：类型|类型`

```typescript
let ddd : string | number
ddd = 'nihao'
console.log(ddd.length)//ddd被推断成了 string，访问它的 length 属性不会报错
console.log(`联合类型${ddd}`)
ddd = 255
console.log(`联合类型${ddd}`)
console.log(ddd.length)//报错 ddd被推断成了 number，访问它的 length 属性时就报错
```

##### 	12.交叉类型

​		交叉类型是将多个类型合并为一个类型。 这让我们可以把现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性。

```typescript
function extend<T, U>(first: T, second: U): T & U {
    let result = <T & U>{};
    for (let id in first) {
        (<any>result)[id] = (<any>first)[id];
    }
    for (let id in second) {
        if (!result.hasOwnProperty(id)) {
            (<any>result)[id] = (<any>second)[id];
        }
    }
    return result;
}

class Person {
    constructor(public name: string) { }
}
interface Loggable {
    log(): void;
}
class ConsoleLogger implements Loggable {
    log() {
        // ...
    }
}
let jim = extend(new Person("Jim"), new ConsoleLogger());
let n = jim.name;
jim.log();
```

##### 13.类型保护

​	`typeof`  直接在代码里检查类型

```typescript
function padLeft(value: string, padding: string | number) {
    if (typeof padding === "number") {
        return Array(padding + 1).join(" ") + value;
    }
    if (typeof padding === "string") {
        return padding + value;
    }
    throw new Error(`Expected string or number, got '${padding}'.`);
}
```

##### 14.泛型

```typescript
function identity<T>(arg: T): T {
    return arg;
}
```

​	函数 **identity**被称作泛型，泛型可以对参数和返回值之间设置关联约束。

#### 二、函数

​	不同与js，可设置返回值的类型，通过泛型可灵活定义参数和返回值的类型。

##### 	1.可选参数

​		参数后加？，参数不能同时设置为可选和默认

```typescript
function f4(age:number, cm?:number) : string {
    //cm为可选参数，可传可不传
    if (cm) {
        return `可选参数------身高为${cm}厘米`;
    } else {
        return `可选参数-----年龄${age}岁`
    }
}
```

##### 	2.剩余参数

​		用于不确定参数数量或者很多参数时:    `...变量名`,也可用来拆分数组

```typescript
function f6(...rest:number[]) : number[] {
    return [...rest];
}
console.log(f6(1,2,3,4,5,6,7,8,9))
 
function f7(a:number, b:number, ...rest:number[]) : number[] {
    return [a, b, ...rest]
}

```

#### 三、接口和类

​	类似与java，类的继承，接口的实现。接口有函数接口，属性接口等对方法和数据类型起到约束作用，接口也可继承接口。方法的重载（同一类中方法名相同，参数不同）和重写（子类中，方法名和参数相同，权权限相比父类更加严格）等。

​	抽象类要被子类继承，接口要被类实现。
​	抽象类中可以声明和实现方法，接口只能申明。
​	接口抽象级别更高
​	抽象类用来抽象类别，接口用来抽象功能。

##### 	1.访问修饰符

`private`:只有当前类中可访问，子类外部类不可访问。

`	public`:公共，都可访问。

`protected`:只有当前类和子类可以访问。无法在new实例内部调用。

`readonly `：只读，不能被修改。

`static`:静态部分，表示类本身的属性和方法，实例中要使用前面需加类名。在一个静态方法中同一个类中有其他静态方法可以通过this			   调用。

#### 四、类型断言

​	使用<>或as来手动指定一个值的类型，不可断言成联合类型。

#### 五、This

##### 	1.箭头函数

​		箭头函数在箭头函数创建的地方获得this。

##### 	2.默认绑定（独立函数调用）

​		此时的this指向全局对象。

```typescript
let number = 1;
function baz():void {
    console.log(this.number);
}
baz(); // 1
```

##### 	3.隐式绑定（对象方法调用）

​		需要考虑的规则是调用位置是否有

[上下文对象]: https://blog.csdn.net/u013510614/article/details/51920840

，且需要注意绑定的对象，否则即**隐式丢失**，this指向全局对象。

```typescript
function baz():void {
    console.log(this.number);
}
let object = {
    number: 1,
    baz: baz
};
object.baz(); // 1

let obj2 = {
    a: 2,
    foo: foo
};
let bar = obj2.foo; // 函数别名！
let a = "oops, global"; // a 是全局对象的属性
bar(); // "oops, global"
```

##### 	4.显示绑定

​	`call(...)`,`apply(...)`可以指定this的绑定对象

```typescript
function foo() {
    console.log(this.a);
}
let obj = {
    a: 2
};
foo.call(obj); // 2
```

##### 	5.new绑定

​	使用`new`来调用函数，或者说发生构造函数调用时，会执行下面的操作。

1. 创建或者说构造一个全新的对象。
2. 这个新对象会被执行`[[Prototype]]`连接。
3. 这个新对象会绑定到函数调用的this。
4. 如果函数没有返回其他队形，那么new表达式中的函数调用会自动返回这个新对象。

```typescript
function baz(number) {
    this.number = number;
}
let bar = new baz(1);
console.log(bar.a); 
```

##### 6.this 绑定的优先级

​	1.函数是否存在new绑定调用：如果是的话this绑定到新创建的对象上。

​	2.函数是否通过`apply`、`call`、`bind`显示绑定：如果是的话，this绑定到指定对象上。

​	3.函数是否在对象方法隐式调用：如果是的话，this绑定到调用对象。

​	4.如果上面三条都不满足的话：在严格模型下，this绑定到undefined，在非严格模式下，this绑定到全局对象上。



##### 六、内存和垃圾回收

​		引用类型和基础类型的存储方式不同。基础类型将值存储在栈内存（先进后出）中。引用类型将指针存储在栈内存中，通过指针引用指向堆内存中对应的对象。涉及引用类型的深拷贝和浅拷贝。

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210326174808464.png" style="zoom: 80%;" />

​		为了节约内存和资源回收，当内存中的变量在任何上下文中都未被引用时，会被标记清理，垃圾回收装置会做一次**内存清理**，销毁带标记的所有值并回收内存。闭包会导致内存泄漏，使变量一直不被标记清理。为促进内存回收，全局对象及全局对象的属性和循环利用都应适时解除。

##### 七、迭代器

​		`for..of`迭代的是对象的值的列表，`for...in`迭代的是对象的键的列表。

##### 八、模块

​		导出`export`和导入`import`,应尽可能地在顶层导出

##### 九、声明合并

​		声明同名接口，命名空间，类等合并。