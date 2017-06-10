# node
node一些常用的方法
[node中文文档](http://nodejs.cn/api/)

###变量声明关键字
- let
```
1.let变量声明不能预解析(预解析就是先声明后解析)
2.let作用域在块级作用域内有效
3.同一个作用域内let不可以声明重名的变量
4.在代码块(为代码块{})内部,不可以声明变量之前使用
```

- const(常量)
```
1.和let之前的4个条件都适用
2.const声明的变量不可以重新赋值
3.必须在声明的时候进行初始化(const abc = 123)
```

- 数组的解构赋值
```javascript
let [a,,c] = [123,456];  //console.log(a,c)//123,undefined
let [a=1,b,c] = [,123,] ; console.log(a,b,c) //1,123,undefined
```

- 对象的解构赋值
```
let {name:usname='lisi',age} = {age:18}; console.log(usname,age)//lisi,18
```

- 字符串解构赋值
```
let [a,b,c,d,e,f] = "hello";console.log(a,b,c,d,e,f) //h e l l o undefined 
```

###对字符串处理的一些方法

- includes判断字符串中是否包含特定的子串,返回布尔值
`
let str = 'hello world'; console.log(str.includes('world',6));//第一个参数为字符串中有没有这个子串,第二个参数为从第几个子串开始查看
`

- startsWith 判断字符串是否以特定的子串开始
- endsWidth 判断字符串是否以特定的子串结束
`参数和上面的inckudes相同`

- 模板字符串
```
let arr = {
	name:'kk',
	age:18
};
function foo (){
	return '我就是我,死颜色不一样的烟火'
}
//模板字符串中,不仅可以使用函数的调用,同时可以使用数值的运算
var tag = `
	<div\>
	<span>${arr.name}</span>
	<span>${arr.age}</span>
	<span>${1+2}</span>
	<span>${foo()}</span>
	</div>
`
console.log(tag)
```

###函数的解构赋值
```
function foo({name = 'kk',age = 12}){
	console.log(name,age)
}
foo({name : 'zhangsan',age : 13});//这里的值可以替换掉函数原来的参数
里面还有个参数...rest参数
function foo(a,...rest){
	console.log(rest)//输出的是剩余的参数,以数组的形式展现的
}
foo(1,2,3,4,5,6)//[1,2,3,4,5,6]
```

- 箭头函数
> 1.函数中的this是声明时的对象,不是调用时的对象
> 2.箭头函数不可以new,也就是说它不是构造函数
> 3.函数内部不可以使用arguments,可以用rest参数替代,也是一个数组

```
1.有一个参数的时候
let foo = value => return value ;
2.没有参数的时候
let foo = () => console.log('hello');
3.有多个参数的时候
let foo = (a,b) => console.log(a,b);
4.有多个事件的时候
let foo = (a,b) => {console.log(a,b);return a+b;}
```

- 类(本质上就是构造函数)
> 类的基本用法
> 静态方法必须要通过类名进行调用,不可以通过实例对象来调用

```
class Person{
	//创建静态方法
	static showinfo(){
	console.log('hello')
}
	constructor(sex,weight){//创建构造函数
	this.sex = sex;
	this.weight = weight
}
	showweight(){//在原型中添加方法
	console.log(this.weight)
}
}
let p = new Person("kk","75kg");
p.showweight()//调用原型的方法
Person.showinfo();//调用静态方法
```

- 函数中扩展运算符的用法
```
1.
let arr = [1,2,3,4,5];
function foo(a,b,c,d,e){
	console.log(a,b,c,d,e)
}
foo(...arr);//将数组中的数据,当做参数进行传递,和数组中的apply用法相同:foo.apply(null,arr)
2.数组的合并
let a = [1,2,3];
let b = [4,5,6];
let arr = [...a,...b]; //[1,2,3,4,5,6]将数组进行合并
```

- 对字节的处理Buffer
1. 构造方法
```
let buf = new Buffer([1,2,3]);  //十六进制  1,2,3
let buf = Buffer.alloc('abc');  //00 00 00    常用
let buf = Buffer.from([1,2,3]);  // 和new的形式相同
```

2. 静态方法
```
let buf = Buffer.alloc(5);
console.log(Buffer.isBuffer(buf));//判断参数中的数据是否为Buffer实例
cosole.log(Buffer.isEncoding('base64'))//判断Buffer是否支持该编码
```

3. 实例方法
> write参数:一:要写入buf的字符串
> 	    二:开始写的位置
> 	    三:写入的长度
> 	    四:编码形式
> 还有的方法可以判断位置以及Buffer是否相同buf.equlas(buf1);//判断两个Buffer实例是否相等
>  var buf = Buffer.from('this is a buffer');   buf.indexOf('buffer');//检索特定的字符串在整个Buffer中的位置

```
let buf = Buffer.alloc(5);//00 00 00 00 00 形成空的编码,可以写入
buf.write('abc',2,2,'utf8');//Buffer实例中写入数据
console.log(buf);
```

- path路径
```
1. basename获取文件名
const path = require('path');
let ret = path.basename('/foo/bar/baz/asdf/quux.html');//截取路径中的文件名
let ret1 = path.basename('/foo/bar/baz/asdf/quux.html','.html');
console.log(ret); //quux.html
console.log(ret); //quux
2. parse将字符串形式转换成对象形式的路径解析
let ret = path.parse('/foo/bar/baz/asdf/quux.html');
console.log(ret); //{文件路径的解析对象}
3. format将对象形式转换成字符串,和parse功能相反
4.  normalize标准化路径,将不全不规范的路径进行规范化
5.  join将字符串拼接成路径; path.join(__dirname,"./README.md");  // 当前路径/README.md
6.  __dirname当前文件路径 
```