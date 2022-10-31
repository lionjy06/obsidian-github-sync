## 타입스크립트란?
타입스크립트는 js와 호환 가능한 프로그래밍 언어로 js에는 없는 정적 프로그래밍을 지원한다. 그렇다면 정적 프로그래밍 이란 무엇일까? 정적 프로그래밍은 타입을 입력하여 프로그래밍 하는 방식의 프로그래밍 언어로 변수, 함수(인자,파라미터) 등에 타입이 입력되어 변수 그자체의 타입이나 함수의 결과값에 대한 타입을 지정 받기도 한다. 

### 타입지정 안하고 프로그래밍 하면 더 편하잖아?
부정하지 않겠다. 분명 더 편하다. 하지만 편하것이 전부이다. 타입을 지정하여 프로그래밍을 하게되면 협업에 있어 일어날수 있는 오류를 사전에 방지할수있고 컴파일(프로그래밍 언어를 기계가 알아들을수 있는 기계어로 변환하는 과정)을 해야한다는 단점이 있지만 그만큼 더 빠르게 기계가 읽어온다. 성능면에서나 협업적인면 디버깅과 같은 부분에 있어서는 정적 프로그래밍이 더 좋다. 그래서 동적 프로그래밍이 가능한 js에서 타입 베이스인 ts로 옮겨가는 추세이다.


### ts에서 제공하는 타입
기본적으로 ts에서 제공하는 타입은 string, number, boolean, undefined, null, array, object, 그리고 tuple이 있다.
tuple은 생소할법도 하니 한번 설명하고 넘어가겠다. tuple은 대게 배열타입에 쓰이는데 배열의 인덱스마다 어떤 타입이 들어와야하는지 까지 지정할수있다.


### object 타입의 데이터를 입력하려 하면 매번 같은 object타입이라도 타입을 전부다 써줘야해??

#### interface: 타입 재사용 가능

사실 처음엔 나도 뭔지몰랐으니까 매번 똑같은 타입의 obj라도 다 썻줬다. 근데 안그래도 된다는걸 깨달았다... 
interface를 사용하면 반복적인 타입 입력을 하지않아도 된다.

```
const thor:{name:string, age:number} = {
	name:thor,
	age:1000
}

const capt:{name:string, age:number} = {
	name: capt,
	age:80
}
보다시피 똑같이 name과 age라는 속성값에 대한 타입이 반복된다. 귀찮다... 그럼 어케해야되냐

interface Hero{
	name:string;
	age:number
}
interface로 선언한뒤 이번엔 선언한 interface를 타입에 넣어주겠따.

const thor:Hero = {
	name:thor,
	age:1000
}

const capt:Hero = {
	name:capt,
	age:80
}

하나만 더 소개하자면 interface는 extends 가 가능하다.

interface AdvanceHero extends Hero {
	weapon:string
}

const batman:AdvanceHero = {
	name:"thor",
	age:44,
	weapon:"bat car"
}
요런식으로 확장받아 사용도 가능

```


#### enum: 정해진 값만 들어오면 좋을때 사용하면 좋음
코로나를 예로 들면 코로나 상태 체크에 음성,양성,회복 과 같이 세개만 상태값으로 들어오면 좋을것 같은 프로그래밍이 있다.
```
enum CovidStatus {
	Positive = 'positive',
	Negative = 'negatice',
	Recover = 'recover'
}

const CovidStatus = (status:CovidStatus) => {
	status = CovidStatus.positive or negative or recover
	return status
}
```


#### 타입을 유연하게 사용할수있나?
Generic: 타입을 유연하게 사용할수있게 해주지만 any는 아니다.
```
interface abc<T,N> {
	name:T;
	age:N
}

const example1:abc = <string,number>(a:string,age:number) => {
	name:'jin',
	age:29 
}

```
그럼 왜 any가 아니고 generic을 쓰지?? 똑같이 유연하게 어떤타입이 들어와도 사용가능한거 아냐?? 
=> ts는 intelligence를 지원한다. intelligence란 예상되는 타입의 메소드를 바로 사용할수있게 해주는데 any는 intelligence를 사용할수없고 generic으로 하면 intelligence이 사용가능하여 개발자 생산성이 훨씐높아진다.


#### type inference (타입추론)
ts는 타입을 지정해주지 않더라도 스스로가 어느정도의 타입추론을 하여 어떤 타입이 들어올지에 대해 예상할수있다. 

#### mapped type
맵드 타입은 타입이나 인터페이스를 한번 훑으면서 인터페이스나 타입에 있던 속성의 타입을 정해줄수있다. 제네릭과 함께 사용하여 유틸리티 타입으로 바꿀수도 있고 그냥 타입을 사용할수도 있다.

```
interface Brad {
	name: 'brad',
	age: 29,
	position: 'server'
} 

type RiseInfo<T> = {
	readonly [P in keyof T]: T[P] 
}
let RiseOfBrad: RiseInfo<Brad> = {
	readonly name:"brad",
	readonly age:29,
	readonly position: 'server'
}

RiseOfBrad.name = 'hohoho' 는 에러를 이르킴 왜냐? 맵드 타입으로 readonly로 만들었기 때문
```