# ES2015 Learn Note
--- 
###Arrows and Lexical This

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
###Classes

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
###Enhanced Object Literals

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
###Template String

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
---
###Destructuring

```
1. 리스트매칭
2. 오브젝트매칭
3. 매칭실패처리
4. function parameter로의 활용
```

~~~javascript
// 리스트매칭
let [a, ,b] = [1,2,3];
a === 1; // true
b === 3; // true

// 오브잭트매칭 & 숏컷
// 아래의 경우 `a`, `b`, `c`에 각각 값들이 들어감
let { op: a, lhs: {op:b}, rhs:c} = getASTNode();

// 당연히 이런코드도 가능 `op`, `lhs`, `rhs` 변수사용
let {op, lhs, rhs} = getASTNode();


// 매칭실패처리
let [a] = [];
a === undefined; //true 매칭되는항목이없을시..

let [a = 1] = [];
a === 1; //true defualt값을 지정했을시 undefined일때 설정한값으로 셋팅됨.


// function parameter로의 활용

let g = ({name: x}) => {
	console.log(x);
};
g({name: 'JungleKim'}); // print JungleKim

//parameter + default
let r = ({x, y, w = 10, h = 10}) => {
	return x + y + w + h;
};
r({x:1, y:2}) === 23; // true


// Point1. 두개의 변수 값 서로 체인지
[a, b] = [b, a];

~~~
---

###Function Parameter 

```
1. default설정 가능
2. 가변인자
3. 배열을 각인자로전달
```

~~~javascript

// default 설정가능
let f = (x, y=12) => {
	return x + y;
};
f(3) == 15; // true

// 가변인자
let ff = (x, ...y) => {
	// y는 Array 객체
	return x * y.length;
};
ff(3, 'hello', true) == 6; // 이경우 y는 ['hello', true]

// 배열을 각인자로전달
let fff = (x, y, z) => {
	return x + y + z;
};
fff(...[1,2,3]) === 6 // true
~~~
---

###Let + Const
```
1. 백문이불여일견
```

~~~javascript

let x; // undefined
{
	// 스코프가다름
	conse x = "sneaky";
	x = "foo"; // error: const는 최초 1회만 값 설정가능
}
x = "bar" // let변수임
let x = "inner"; // error: 이미 선언된 x임 

// 특징정리끝... 참쉽죠?
~~~
---

### Iterators + For...Of
```
1. For...Of구문의 특징
	- 타 언어의 ForEach구문과 동일
	- 오브젝트의 속성이 아닌 data를 iterator하는 구문 
	- Array.forEach와 달리 break, continue, return사용가능
	- 문자열 그리고 ES6에 추가된 Sets와 Map 순회가능
2. Iterator구현하기
```
~~~javascript

// {value, done}을 리턴해주는 next를 속성으로 가지는 Symbol.iterator Object를 구현해야한다.

// 아래코드는 iterator로 구현한 fibonacci코드이다
let fibonacci = {
	[Symbol.iterator]() {
		let pre = 0, cur = 1;
		return {
			next() {
				// pre,cur은 상단의 pre, cur객체이다.
				[pre, cur] = [cur, pre+cur];
				// done을 항상 false로 내려주기떄문에 무한루프이다.
				// for...of 로직안에서 분기처리해줘야함.
				return {done: false, value: cur};
			}
		};
	},
};

for (var n of fibonacci) {
	if (n > 1000) {
		break;
	}
	console.log(n);
}

~~~
---
### Generator
```
1. Iterator와 똑같이 Symbol.iterator를 구현
2. generator함수는 function이 아닌 function*구문으로 시작
3. generator함수안엔 return과 비슷한 역활을 하는 yield구문이 존재
4. generator함수의 특징들
	- 특징1: yield는 여러번 호출이가능
	- 특징2: yield는 함수의 동작을 멈춤, 브레이크포인트걸린거마냥 멈추멈춘다고 생각하면 편함
	- 특징3: next를 호출할때마다 멈춰있던함수는 다음 yield가 나올떄가지 진행
	- 특징4: next의 리턴값은 iterator와같은 {done, value}임
```

~~~javascript

// Generator를 이용해 fibonacci구현
let fibonacci = {
	[Symbol.iterator]: function*() {
		let pre = 0, cur = 1;
		
		while(1) {
			[pre, cur] = [cur, pre+cur];
			yield cur;
		}
	},
};

for (var n of fibonacci) {
	if (n > 1000) {
		// Generator가 무한루프이기때문에 break구현
		break;
	}
	console.log(n)
}

// Generator원리 눈으로 확인하기
function* helloGenerator(name) {
  yield "Hello Generator! - " + name;
  yield "ES6 is very awesome!!!";
  yield "see you later!";
}

let generator = helloGenerator("JungleKim");
// [object Generator]
generator.next();
// {value: "Hello Generator! - JungleKim", done: false}

generator.next();
// {value:"ES6 is very awesome!!!", done: false}

generator.next();
// {value:"see you later!", done: false}

generator.next();
// {value:undefined, done: true}

// 위의 코드의 흐름을 보면 특징에 써놨던 멈춘다 라는 것의 의미가 조금은 와닿을 것같다.

// 만약 위의 next들을 계속적으로 호출하는게 싫고 result들을 한방에 얻고싶다면?
let results = yield* helloGenerator('JungleKim');


// 이런것도 가능하다는점
function* g3() {
  yield* [1, 2];
  yield* "34";
  yield* Array.from(arguments);
}

var iterator = g3(5, 6);

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: "3", done: false }
console.log(iterator.next()); // { value: "4", done: false }
console.log(iterator.next()); // { value: 5, done: false }
console.log(iterator.next()); // { value: 6, done: false }
console.log(iterator.next()); // { value: undefined, done: true }

~~~
---
###Unicode
```
1. ES6부터 지원한다...
2. 유니코드를 지원하지않았을 때엔 Unicode를 분해하여 표현.
```
---
###Modules
```
1. ES5엔 Commonjs 혹은 AMD를 이용해서 Module관리를 했음.
2. ES6엔 Language Level에서 Module관리를 지원한다!!
```

~~~javascript
// lib/customMath.js
const moduleName = 'customMath';
const sqrt = Math.sqrt;
let sum = (...numbers) => {
	let result = 0;
	for (number of numbers) {
		result += number;
	}
	return result;
};

let square = (x) => {
	return x*x;
};

let diag = (x, y) => {
	return sqrt(square(x) + square(y));
};

export default moduleName; // 모듈당 딱하나 function or class or let변수
export { sum, square, diag };
export sqrt;

// 추가
// export function foo() { ... } 도 가능
// export class foo { ... } 도 당연히가능
// export let name="junglekim", name2="junglekim1";도 가능

////

// example1 main.js
import { square, diag } from 'lib/customMath';
console.log(square(10));
console.log(diag(4,3));

// example2 main.js
import * as lib import 'lib/customMath';
console.log(lib.square(10));
console.log(lib.diag(4,3));

// example3 main.js
// export default를 임포트할때는 import시의 이름은 상관없음 _이 아니라 asdf도됨
import _, { square as sq, diag as dg } from 'lib/customMath';
console.log(_); // customMath출력;
console.log(sq(10);
console.log(dg(4,3));

~~~








