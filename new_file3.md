一个Generator函数与普通function的区别就是函数名前面多了一个星号 * 但是执行时有很大不同，与yield命令配合，可以实现暂停执行的功能
```
let go = function*(x) {
      console.log('x', x);
      let a = yield x;
      console.log('xx', x);

      console.log('a', a);

      let b = yield (x + 1) + a;

      yield a + b;

      console.log('a + b =', a + b);

      return a + b;
    }
    go(10);
```
先执行go(10)

这一行代码，发现console是空的，所以说明 generator 函数被直接执行时是没有执行函数体内的代码的，否则应该执行 console.log('x', x) ，要让代码执行，需要通过 next() 函数

修改代码如下
```
let go = function*(x) {
      console.log('x', x);
      let a = yield x;
      console.log('xx', x);

      console.log('a', a);

      let b = yield (x + 1) + a;

      yield a + b;

      console.log('a + b =', a + b);

      return a + b;
    }
    go(10);
    let g = go(10);
    console.log(g.next());
```
### 第一个next不能传值，因为一开始还没有断点，也就是没有yield命令。这里的g变量，我个人把它称作“断点对象”，便于描述，可以是其他的值，例如：go(20)。next是断点对象的方法，每次执行到下一个yield命令为止，然后返回断点位置yield后面的表达式的值。
```
结果为

x 10
Object {value: 10, done: false}
Object {value: 10, done: false} 就是yield的返回值，返回的是一个对象，包括 value 和 done 两部分，value自然是返回值，done是该generator函数是否执行完，这里看到false，就是表明该函数还有后续可以执行的部分
```
实际上，next函数的参数是赋值给关键字yield后面的整个表达式
```
let go = function*(x) {
      console.log('x', x);
      let a = yield x;
      console.log('xx', x);

      console.log('a', a);

      let b = yield (x + 1) + a;

      yield a + b;

      console.log('a + b =', a + b);

      return a + b;
    }
    go(10);
    let g = go(10);
    console.log(g.next());
    console.log(g.next(1000).value);
x=10，a=1000，到let b = yield (x + 1) + a;结束，此时断点对象的值是b=(x+1) + a，也就是1011
```
```
结果

x 10
{ value: 10, done: false }
xx 10
a 1000
1011
```
接下来调用next(50)，b=50,a不变为1000，到yield a + b;结束，此时断点对象的值是a + b，也就是1050

然后再执行next(8)，之后已经没有断点，但还没有执行完，只是没有yield命令，所以不接受8这个参数，断点对象的值还是上一次的值，但此时已经执行完成，返回true

最后再执行一次next(8)，已经执行完，value是undefined
```
let go = function*(x) {
      console.log('x', x);
      let a = yield x;
      console.log('xx', x);

      console.log('a', a);

      let b = yield (x + 1) + a;

      yield a + b;

      console.log('a + b =', a + b);

      return a + b;
    }
    go(10);
    let g = go(10);
    console.log(g.next());
    console.log(g.next(1000).value);
    console.log(g.next(50));
    console.log(g.next(8));
    console.log(g.next(8));
```
```
下面是运行结果

x 10
{ value: 10, done: false }
xx 10
a 1000
1011
{ value: 1050, done: false }
a + b = 1050
{ value: 1050, done: true }
{ value: undefined, done: true }
```
作者：带头大哥orz
链接：https://www.jianshu.com/p/651922749334
來源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。