```javascript
const demo = new Promise((resolve, reject)=>{
	setTimeout(()=>{
		resolve('hello from executor')
	}, 3000)
})
demo.then(console.log, console.err)
```

观察者模式；
收集依赖-触发通知-取出依赖执行
	定义一个*executor*
	使用then/catch定义executor完成或失败时的callback
	executor执行后，调用对应的callback

```javascript
class _Promise{
	
	constructor(executor){
		this._resolveQueue = []
		this._rejectQueue = []
		this._resolve  = (data)=>{
			while(this._resolveQueue.lenth){
				this._resolveQueue.shift()(data)
			}
		}
		this._reject  = (data)=>{
			while(this._rejectQueue.lenth){
				this.rejectQueue.shift()(data)
			}
		}
		executor(_resolve, _reject)
	}
	then(resolve, reject){
		this.resolveQueue.push(resolve)
		this.rejectQueue.push(reject)
	}
}
```

遵循Promise/A+规范
- promise的状态: pending - fulfilled - rejected
- promise的状态变更：pending -> fulfilled | pending -> rejected
- then返回promise及链式调用

