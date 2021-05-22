# Promise js实现

## 1.Promise（待优化）

```javascript
  const PENDING='PENDING';
  const FULFILLED='FULFILLED';
  const REJECTED='REJECTED';

  const resolvePromise=(promise,x,resolve,reject)=>{
    //返回重复引用的错误
    if(promise===x){
      return reject(new Error('!!'))
    }
    if(x instanceof MyPromise){
      // x属于MyPromise对象调用then方法返回 resolve, reject
      // x.then(value => resolve(value), reason => reject(reason)))
      x.then(resolve,reject)
    }else{
      resolve(x)
    }
  }

  class MyPromise{
    constructor(exector){
      
      //初始化状态
      this.status=PENDING;
      this.value=undefined;
      this.reason=undefined;
      //存放成功和失败的回调
      this.onResolvedCallback=[];
      this.onRejectedCallback=[];

      //调用成功函数
      let resolve=value=>{
        if(this.status === PENDING){
          this.status=FULFILLED;
          this.value=value;
          // 判断成功回调是否存在，如果存在就调用
          // 循环回调数组. 把数组前面的方法弹出来并且直接调用
          // shift方法是返回数组的第一项，会改变原数组，相当于在数组中每次弹出第一项
          // 数组变空了说明所有回调都执行了
          while(this.onResolvedCallback.length) this.onResolvedCallback.shift()(value);
        }
      }
      //调用失败函数
      let reject=reason=>{
        if(this.status===PENDING){
          this.status=REJECTED;
          this.reason=reason;
          while(this.onRejectedCallback.length) this.onRejectedCallback.shift()(reason);
        }
      }
      //立即执行
      exector(resolve,reject);
    }

    //then方法
    then(fufilledCallback,rejectedCallback){
      let promise=new MyPromise((resolve,reject)=>{

        if(this.status===FULFILLED){
          //通过setTimeout宏任务模拟微任务
          setTimeout(()=>{
            try{
              let x=fufilledCallback(this.value);
              resolvePromise(promise,x,resolve,reject)
            }catch(err){
              reject(err)
            }
          },0)
        }else if(this.status===REJECTED){
          setTimeout(()=>{
            try{
              let x=rejectedCallback(this.reason);
              resolvePromise(promise,x,resolve,reject)
            }catch(err){
              reject(err)
            }
          },0)
        }else{
          this.onResolvedCallback.push(()=>{
            setTimeout(()=>{
              try{
                let x=fufilledCallback(this.value);
                resolvePromise(promise,x,resolve,reject)
              }catch(err){
                reject(err)
              }
            },0)
          })
          this.onRejectedCallback.push(()=>{
            setTimeout(()=>{
              try{
                let x=rejectedCallback(this.reason);
                resolvePromise(promise,x,resolve,reject)
              }catch(err){
                reject(err)
              }
            },0)
          })
        }
      })
      return promise
    }
  }
```

## 2.Promise.finally

```javascript
Promise.prototype.myFinally=function(callback){
    return new Promise((resolve,reject)=>{
      this.then((res)=>{
        callback();
        resolve(res)
      },(rea)=>{
        callback();
        reject(rea)
      })
    })
  }
```

