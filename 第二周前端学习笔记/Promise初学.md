#### 20.Promise
**Promise是异步编程的一种解决方案**，其实是一个构造函数，自己身上有**all、reject、resolve**这几个方法，原型上有**then、catch**等方法。

特点：
（1）对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：**pending（进行中）、fulfilled（已成功）和rejected（已失败）**。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。

（2）**一旦状态改变，就不会再变**，任何时候都可以得到这个结果。Promisce对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为**resolved**（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。（与事件（Event）完全不同）

##### 1.参数
Promise中的参数executor是一个执行器函数，它有两个参数**resolve**和**reject**。它内部通常有一些异步操作，如果异步操作成功，则可以调用resolve()来将该实例的状态置为fulfilled，即已完成的触发then()方法，如果一旦失败，可以调用reject()来将该实例的状态置为rejected，即失败的触发catch()方法。

异步函数不用调用就自动执行，例：

    let p = new Promise(function(resolve,reject){	
        //做一些异步操作		    
        setTimeout(function(){	   
            console.log('执行完成');
            resolve('要返回的数据可以任何数据例如接口返回数据');	
    }, 2000);});

结果：2秒后，输出“执行完成”，并且调用resolve方法。

##### 2.then()
Promise对象含有then方法，then()调用后返回一个Promise对象，意味着实例化后的Promise对象可以进行链式调用（进行下一步），而且这个then()方法可以接收两个函数，一个是处理成功后的函数，一个是处理错误结果的函数(通常交给catch()处理）。两个函数都接收promise传出的值作为参数。例：

    {
      let ajax=function(){
        console.log('执行3');
        return new Promise(function(resolve,reject){
          setTimeout(function () {
            resolve()
          }, 1000);
        })
      };
    
      ajax()
        .then(function(){
        return new Promise(function(resolve,reject){
          setTimeout(function () {
            resolve()
          }, 2000);
        });
      })
        .then(function(){
        console.log('timeout3');
      })
    }
    {
      let ajax=function(num){
        console.log('执行4');
        return new Promise(function(resolve,reject){
          if(num>5){
            resolve()
          }else{
            throw new Error('出错了')
          }
        })
      }
    
      ajax(6).then(function(){
        console.log('log',6);
      }).catch(function(err){
        console.log('catch',err);
      });
    
      ajax(3).then(function(){
        console.log('log',3);
      }).catch(function(err){
        console.log('catch',err);
      });
    }

链式调用特点：

- 每次的 then 都是一个全新的 promise，默认 then 返回的 promise 状态是 fulfilled
- 每次的 then 都是一个全新的 promise，不要认为上一个 promise状态会影响以后then返回的状态
- then 是对上个promise 的rejected 的处理，每个 then 会是一个新的promise，默认传递 fulfilled状态
- 如果内部返回 promise 时将使用该 promise
- 如果 then 返回promise 时，后面的then 就是对返回的 promise 的处理，需要等待该 promise 变更状态后执行。
- 如果then返回 promise 时，返回的promise 后面的then 就是处理这个promise 的
- 如果不 return 情况就不是这样了，即外层的 then 的promise 和内部的promise 是独立的两个promise

优势：可以在then方法中继续写Promise对象并返回，然后继续调用then来进行回调操作。比传递callback函数要简单、灵活的多。

##### 3.catch()
catch就是用来捕获异常的，也就是和then方法中接受的第二参数rejected的回调是一样的。例：


    function promiseClick(){
		let p = new Promise(function(resolve, reject){
			setTimeout(function(){
				var num = Math.ceil(Math.random()*20); //生成1-10的随机数
				console.log('随机数生成的值：',num)
				if(num<=10){
					resolve(num);
				}
				else{
					reject('数字太于10了即将执行失败回调');
				}
			}, 2000);
		   })
		   return p
	   }

	promiseClick().then(
		function(data){
			console.log('resolved成功回调');
			console.log('成功回调接受的值：',data);
		}
	)
	.catch(function(reason, data){
		console.log('catch到rejected失败回调');
		console.log('catch失败执行回调抛出失败原因：',reason);
	});	

优势：如果抛出异常了（代码出错了），那么并不会报错卡死js，而是会进到catch方法中。把错误原因传到了reason参数中。即便是有错误的代码也不会报错了。

##### 4.all()
all接收一个数组参数，这组参数为需要执行异步操作的所有方法，里面的值最终都算返回Promise对象。例：

    //new 三个promise并封装在三个函数中
    Promise
        .all([promiseClick3(), promiseClick2(), promiseClick1()])
		.then(function(results){
			console.log(results);
		});

all会把所有异步操作的结果放进一个数组中传给then，然后再执行then方法的成功回调将结果接收。

优势：以用all并行执行多个异步操作，并且在一个回调中处理所有的返回数据，比如你需要提前准备好所有数据才渲染页面的时候就可以使用all,执行多个异步操作将所有的数据处理好，再去渲染。

注意：
1. 任何一个 Promise 执行失败就会调用 catch方法
2. 可以将其他非promise 数据添加到 all 中，它将被处理成 Promise.resolve
3. 如果某一个promise没有catch 处理，将使用promise.all 的catch处理
4. 参数必须是可迭代类型，如Array/Set

##### 5.race()
与all()相反，race()是谁先执行完成就先执行回调。先执行完的不管是进行了race的成功回调还是失败回调，其余的将不会再进入race的任何回调(但是仍会继续执行）。例：

    //new 三个promise并封装在三个函数中
    Promise
        .race([promiseClick3(), promiseClick2(), promiseClick1()])
		.then(function(results){
			console.log(results);
		}，function(reason){
			console.log('失败',reason);
		}););

##### 6.async/await 语法糖
- 通过async 可以指定一个函数为异步函数,执行异步函数后才能使用await。
- await的作用其实就是替代了then方法，将resolve的值直接返回，使用起来更加方便。例：


    async function demo() {
        var result = await test(); //这里可以直接接收到返回值
        return result;//如果调用demo函数的话，也是需要使用await来接收result这个返回值的.
    }

注意：
1. 当return 一个await的值的时候，接收的函数也需要铜鼓await来接收，否则接收的数据会失败。
2. await 后面一般是promise，如果不是直接返回；
3. await 必须放在 async 定义的函数中使用；
4. async/await 本质还是promise，只是更简洁的语法糖书写；
5. async内部发生的错误，必会将promise对象变为rejected 状态，所以可以使用catch 来处理。