# ES2015 Learn Note
--- 
Arrows and Lexical This

##### Arrow

~~~javascript
// 예시코드
let functionName = (param1, param2) => {
	console.log(param1);
	console.log(param2);
};

functionName('JungleKim', 'is awesome');

// Point1. 단일 parameter의 경우 ()생략가능
nums.forEach(v => {
	if (v % 5 === 0)
		fives.push(v);
});
~~~

##### Lexical This 
- Object의 맴버변수로 위치할경우 function안에서 this는 Object와 공유한다.
	
~~~javascript
// 예시코드
let numberic = {
	_value: 100,
	validate() {
		if (this._value === 100)
			console.log("this Value is 100!");
	}
};
numberic.validate();
~~~

---
Classes

```
- 프로토타입 베이스의 객체지향 모델
- 상속 지원
- instance method, static method, constructor지원
```

~~~javascript
// 예시코드
let sharedInstance;
class NetworkManager {
	constructor() {
		this.base_url = 'http://localhost';
	}
	
	post(endpoint, parameters) {
		let url = this._getUrl(endpoint);
		console.log(`request ${url}`);
		return 'response';
	}
	
	_getUrl(endpoint) {
		return this.base_url + endpoint;
	}
	
	static manager() {
		if (!sharedInstance) {
			sharedInstance = new NetworkManager();
		}	
		return sharedInstance;
	}
}

let manager = NetworkManager.manager();
console.log(manager._getUrl('/users'));
~~~
---
Enhanced Object Literals

~~~javascript
// 예시코드
let key = "으베베베베";
let object = {
	// 생성시에 prototype지정가능
	__proto__: protoType,
	
	// super call가능
	toString() {
		return 'object' + super.toString();
	}
	
	// handler: handler, shorthand
	handler,
	
	// 새로운문법 []
	// 변수를 key로 활용가능
	[key]: 'value',
	// 활용 dynamic key
	["property"+ (() => 1234)()]: 'value1',
};
~~~
---
Template String

```
1. multiline기능
2. string subsituation기능
3. tagged template
```

~~~javascript
//예시코드1

//기존 ES5 Multiline
var multilineString = "Foo\
Bar";
// ES6 Template String Multiline
let multilineString2 = `Foo
Bar1;

// 예시코드2
let a=10, b=10;
let name="JungleKim";
let getFoo = () => {
	return "Bar";
};
console.log(`my name is ${name}.`);
console.log(`a + b = ${a+b}`);
console.log(`Foo = ${getFoo()}`);

// 예시코드3
/* 
	Tagged Template란 function을 사용하여 
	template string의 출력을 변형, 가공할수있는 기능입니다.
*/
function tag(literals, ... substitutions) {
	// ex) Input - `Hello ${name}, how are you ${time}?`
	// literals = ["Hello ", ", how are you", "?"]
	// substitutions = ["Bob", "today"]
	
	// 주의할점 literals.length 는 항상 substitutions.length+1
	let result = "";
	
	... Action ...
	
	return result;
}
let name = "Bob", time = "today";
tag`Hello ${name}, how are you ${time}?`

~~~













