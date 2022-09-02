## nest JS 이전의 nodeJs의 개발 방식

간단하게 nodeJs에 대해서 소개를 해보자면 nodeJs는 기존의 JS와는 다른 브라우저 전용 프로그래밍 언어가 아닌 JS기반이지만 서버에서 활용할수 있는 프로그래밍언어 즉 백엔드 프로그래밍언어의 하나이다. 

  

오늘의 주제와는 살짝 벗어나지만 공부를 하면서 꼭한번은 짚고 넘어가면 좋을거같아서 nodeJs를 소개하면서 살짝 쿵 언급해보고 싶은게 있다. 면접을 준비한 사람 대부분은 이미 한번쯤은 봐서 알고있겠지만 nodeJs는 싱글쓰레드 언어이다. 싱글쓰레드 언어는 병렬적으로 프로그래밍을 실행히키는것이 아닌 직렬적으로 한번의 하나의 작업만 수행한다. 뭐든 그러하듯 장단점은 존재한다. 싱글쓰레드는 직렬적으로 작업을 수행하기 때문에 멀티쓰레드에서 일어나는 복수의 작업을 바꿔가며 진행하는 컨텍스트 스위칭이나 작업 동기화(작업 동기화를 안하면 결과값이 원하는것과 다르게 나옴)에 따른 시간을 아낄수있다. 

  

하지만 이렇게도 생각하는 사람이 있을것이다. 싱글쓰레드 작업을 하는건 좋은데 비동기적으로 처리해야할 작업이 생기면 그 작업이 끝날때까지 모든 작업을 중단할거냐?? 직렬적으로 동작한다는건 즉 병목현상이 일어날수 있다는 말과 비슷할수있겠다. 하지만 nodeJs에서 제공하는 또하나의 강력한 기능이 있다. non-blocking I/O이다. non-blocking IO는 비동기작업은 task-que라는 공간에 임시적으로 보관되어있고 동기적으로 처리할일을 마무리한뒤 비동기적으로 처리해야하는일을 처리한다. 최근 당연하다면 당연한건데 놓친 부분이 있어 이부분을 같이 설명하고 싶다. 싱글쓰레드는 빠르고 non-blocking IO는 비동기작업을 처리할수있어, 근데 어쩌라고? 라고 나도 생각했다. 하지만 이것이 전부 이어져있었다는 사실... 싱글쓰레드란 사실 작업을 처리하는것이 아닌 처리해야할 작업을 읽어오는것에 불과한 과정이다. 즉 싱글쓰레드는 읽기전용 이었다는 사실... 그래서 nodeJs가 빠른것이었다. 아무리 직렬로 일을 처리할수있어도 모든 작업을 그때그떄 처리한다면 늦을 수밖에 없다. 하지만 싱글쓰레드가 하는일은 빠르게 해야할 작업을 읽고 진짜로 처리하는것은 non-blocking IO기능으로 업무를 나누고 work node가 처리한다 라고 보면 쉽다. work node는 음... 우리가 흔히 알고있는 event-loop에서 작업의 처리를 위해 관여되는 작은 하나의 기능들이라고 이해하면 쉬울것같다.

  * 노드의 메인쓰레드는 업무를 할당하는 역할로 싱글쓰레드 방식이지만  그 밑 여러 쓰레드가 존재한다. 즉 처리방식은 멀티쓰레드 라고 볼수있다. LIBUV*

TMI가 조금 길었다. 그러면 이제 부터 다시 본론으로 넘어가서 설명을 이어가도록 하겠다. 이전 버전에 대해서 말하자면 끝도 없이 길어지니 현재 우리가 작업하는 프로그래밍 방식에서 가장 커다란 영향을 끼친 버전 부터 설명하겠다. JS에서는 ES6에서 처음으로 소개된 기능인 class는 앞으로 우리가 계속해서 다룰 프로그래밍 방식의 핵심이기 때문에 여기서 부터 설명을 시작하겠다. 

  

클래스는 여러가지 기능이 있다. extends와 super를 이용한 상속이 대표적이 겠지만 우리가 다루고자 하는 내용은 상속이 아니다. 우리는 메소드 라는 클래스에서 정의한 함수기능을 사용하는데 초점을 맞출것이다.

  

앞으로 소개할 singleton, DI, ioc에 대해서 자세히 언급하기전 가장먼저 이것들의 기초라고 할수있는 프로그래밍 작성 방식인 OOP부터 설명하도록 하겠다.

  

OOP란 프로그래밍을 작성하는 방식중 하나로 독립적인 객체(class)를 만들고 그안에서의 properties 또는 method를 활용하여 프로그래밍을 작성할때 코드 재사용성을 높혀 유지보수에 용이하고 코드가 용도에 따라 정리가 잘된다는 장점이 있다.

  
## OOP

OOP의 주요 4가지 특징.

1. 추상화: 추상화는 쉽게 말하면 자동차를 예로 들수있을것 같다. 우리는 자동차가 있고 그것을 운전해서 멀리갈수있지만 정확하게 자동차에 어떤 파츠가 들어가거나 피스톤이 어떻게 작동해서 앞으로 갈수있는지 모른다. 즉 사용자(개발자)는 사용하고자 하는 메소드가 어떤 원리로 결과를 리턴하는지는 알수없지만 어쨋든 사용하는데는 전혀 무관하다는 것으로 이해할수 있을것같다. (js의 내장된 map()또한 어떤식으로 반복문을 돌리는지 내부로직을 알수없지만 우리가 쓰는데는 전혀 문제가 안됀다.)

2. 캡슐화: 캡슐화는 외부에서의 코드 변경을 막기 위한 보안적인 목적을 띄는 특성이다. 클래스 내부의 메소드를 private하게 만들어 메소드 자체는 사용가능 하지만 내부로직을 수정하거나 삭제할수는 없게 만드는 특징 이라고 볼수있겠다.

3. 상속: 상속은 class가 갖는 고유 기능중 하나로 하나의 클래스(자식 클래스)는 또다른 클래스(부모 클래스)로 부터 메소드들을 상속받아 사용할수있게 만들어 주는 특징이다.

4. 다형성: 상속과 연계되는 특징인데 부모 클래스로 부터 상속을 받을때 다형성으로 인해 자식 클래스는 부모 클래스의 메소드는 사용하되 덮어쓰기 형식으로 이름은 같지만 좀더 기능이 추가되거나 수정될수있어 편하게 사용 목적에 따라 메소드를 편집할수있는 기능이라고 보면 될수있을것이다. (메소드 오버라이딩에 대해 조금더 공부해보면 좋을듯!)

OOP의 주요 5가지 원칙

1. 단일책임의 원칙: 하나의 모듈은 하나의 책임만을 가져야 한다. => 쉽게말하면 유저에 관한 객체면 유저에 관한 로직만 담고 있어야하고 상품관련 객체면 상품관련 로직만 담고있어야한다. (여기서 헤깔리지 말아야할껀 유저로직을 작성하면서 상품관련 로직을 가져와서 사용할순있어도 유저객체가 상품만을 위한 로직을 담아서는 안됀다는 뜻이다. => 이런 원칙을 갖는 이유는 서로 관련없는 객체들을 만들어 서로가 서로의 의존성을 갖지 않게 만들겠다는 뜻이다. 필요하면 의존성을 주입해줄순 있지만 기본적으로는 의존도가 낮거나 없어야 유지보수나 확장서에 용이하다.

*티어별 책임을 지는것이 단일 책임의 원칙*

2. 개방 폐쇄 원칙: 확장에 대해서는 열려있되 수정에 대해서는 닫혀있어야 한다. => 내 식대로 설명하자면 그떄그때 필요한 로직이 있다면 로직을 확장시켜서 만들면 된다. 하지만 만들어진 로직을 수정하는건 지양해야한다. 왜냐하면 그 로직이 사용된 서비스에서 그 로직이 수정됨에 따라 어떤 영향을 받을지 모르기 때문이다. 즉 유지보수를 위해서는 수정은 별로 안좋다, 하지만 새로운 비지니스로직을 확장시키는건 문제 없다.

3. 인터페이스 분리 원칙: 클라이언트의 목적과 용도에 적합한 인터페이스만을 제공해야 한다 => 이게 우리가 흔히 경험했던 nestjs에서의 service와 controller의 관계라고 보면 될것같다. 클라이언트는 비지니스로직 자체를 접할 필요가 없다. 추상화에 따라 클라이언트는 내부로직이 어떻게 돌아가는지에 대한 불필요한 정보를 받기보다는 비지니스 로직을 실행시킬수있는 interface정보만 알면되고 이것이 인테페이스 분리 원칙이다.

4. 리스코프 치환 원칙: 하위 타입은 상위 타입을 대체할수 있어야 한다 => 이건 상속에 관한 내용이라고 보면된다. 부모 클래스로 사람이 있고 자식 클래스로 아시아인 이 있다고 쳤을때 자식클래스가 부모 클래스를 상속받아 사람 클래스의 특징을 상속받았을때 자식클래스는 부모클래스가 수행할수있는 일을 그대로 수행할수있어야 한다 라는 뜻이다. => 아시아인 클래스가 사람 클래스를 상속받았을때 사람 클래스가 수행할수있는 것을 못한다는건 말이 안돼기 때문이다. (아시아인도 사람이야..)

5. 의존 역전 원칙: 고수준 모듈(controller)는 저수준 모듈(service)의 구현에 의존해서는 안되며, 저수준 모듈이 고수준 모둘에서 정의한 추상 타입에 의존해야 한다.(올바른 의존성 관계에 관한 원칙) => 추상화에 의존하며 구체화에는 의존하지 않는 설계원칙(쉽게말해 비지니스 로직 자체를 불러내서 프로그래밍 하는것이 아니라 비지니스 로직을 담고있는 인터페이스인 controller를 써야한다는 뜻.)


```

class Animal {

  

  constructor(name) {

    this.speed = 0;

    this.name = name;

  }

  

  run(speed) {

    this.speed += speed;

    alert(`${this.name} 은/는 속도 ${this.speed}로 달립니다.`);

  }

  

  stop() {

    this.speed = 0;

    alert(`${this.name} 이/가 멈췄습니다.`);

  }

  

}

  

class Rabbit extends Animal {

  hide() {

    alert(`${this.name} 이/가 숨었습니다!`);

  }

  

  stop() {

    super.stop(); // 부모 클래스의 stop을 호출해 멈추고,

    this.hide(); // 숨습니다.

  }

}

  

let rabbit = new Rabbit("흰 토끼");

  

rabbit.run(5); // 흰 토끼 은/는 속도 5로 달립니다.

rabbit.stop(); // 흰 토끼 이/가 멈췄습니다. 흰 토끼 이/가 숨었습니다!

  

```

메소드 오버라이딩은 기존 부모 클래스의 stop 메소드에서 this.hide()를 추가하여 사용하기 쉽게 편집했다 라고 보면 쉬울거같다.

  

## 싱글톤 패턴

싱글톤 패턴은 하나의 객체에 하나의 인스턴스만 만들어 두는것이다. 이것의 장점은 메모리 관리에 있다. 한번의 new 호출로 불러온 인스턴스를 재사용할수 있다는 점에서 장점이 있다. 하지만 싱글톤의 단점은 인스턴스를 이루고 있는 로직 자체이 하나라도 바뀌면 해당 객체를 이용하여 만들어진 서비스들 모두 영향을 끼치게 된다. 즉 이말은 너무 강하게 의존성이 결합되어 있어 유지보수에 용이하지 않다는 말과 같다.

  

## Nest JS에서의 DI & IOC

앞서 설명한 싱글톤 패턴은 의존성이 너무 강하게 결합되어 있어 유지보수가 용이하지 못하고 확장성에 약하다 라는 단점이 있었다. 이러한 단점을 해결하기 위해 의존성 주입(dependancy injection)이라는 개념이 도입되었다. 의존성 주입은 말 그대로 필요한 외부의 객체를 가져와 현재 내가 프로그래밍 하고 있는 곳에 사용한다는 개념이다. 개념만 으로 설명하면 이해하기 힘들수 있으니 예시와 함께 조금 풀어서 설명하자면 만약 내가 상품 관련 API를 개발하고 있는데 유저에 관한 로직이 필요하다고 한다면 우리는 유저관련 메소드가 정리되어있는 유저객체를 가져와 사용하면 된다. 이렇게 되면 좋은점이 뭐냐면 상품에서 사용되고 있는 로직은 유저관련 로직을 바로 볼수있다. 즉 사용하고 있는 유저객체 메소드가 변경되었을때 특별한 개발자의 수정 없이도 변경된 정보가 바로 적용된다. 

  *nestjs 객체들간 ioc di를 사용하지만 결국 provider에서는 싱글톤으로 injectable키워드로 컨테이너에  객체를 경정할수있다.* 

   config => 런타임 시점에서 환경변수를 정해서 사용할수있음

```

Product.service.ts

  

@Injectable()

export class ProductService {

constructor(

private readonly userService:UserService)

  

async createProduct(){

const user = await this.userService.findUserById({id:userId})

}

}

```

  

nestJS에서는 이러한 의존성주입을 쉽게할수있는 내장 함수들이 존재한다. 이러한 내장함수를 데코레이터라고 칭하고 객체(클래스)를 생성할 당시 @Injectable() 과 같은 데코레이터로 객체를 의존성을 주입할수있는 대상이 된다. 이렇게 의존성 주입 대상이 된 객체는 provider에 의해 원하는 모듈에 주입이 된다. 

  

그렇다면 이러한 질문을 할수도 잇을것같다. provider는 뭐고 module은 뭐냐?  이것 또한 nestJs에서 제공해주는 기능이다. 기존 DI방식은 의존성을 주입해줄수는 있지만 주입해주는 모든 과정을 사용자(개발자)가 직접했었어야 했다. 하지만 nestJS는 의존성 주입을 받아야하는 객체를 provider라는 키워드안에 넣어주기만 한다면 알아서 의존성을 주입해주었다. 그리고 의존성을 대신해서 주입해주는 provider외에도 import, export, controller등  사용자가 직접했어야 하는 과정을 nestJS가 대신해주는데 이것을 제어의역전(IOC)라고 부른다. 제어가 역전된다는 뜻이 들리기엔 좋게 들리지 않지만 사실은 내가 했어야 하는 귀찮은 일들을 nestJS가 모듈화 하여 대신 해준다는 뜻으로 이해하면 좋을것 같다.

  

지금까지 singleton 패턴과 그것의 단점 그래서 DI가 필요했다 근데 nestJS는 DI를 지원하면서 동시에 의존성을 주입하기 위한 과정을 IOC를 통해 대신 해주기 때문에 굉장히 편리하다. 그래서 기존 nodeJS에서 하던 OOP방식이 nestJS로 넘어가게 되었다. 라고 이해하면 쉬울것같다.


## clean architecture 

Clean architecture is a software design philosophy that separates the elements of a design into ring levels. An important goal of clean architecture is to provide developers with a way to organize code in such a way that it encapsulates the [business logic](https://whatis.techtarget.com/definition/business-logic) but keeps it separate from the delivery mechanism. 

![[Pasted image 20220901144014.png]]

- 클린 아키텍쳐란 소프트웨어 디자인 철학으로 디자인의 요소들을 링형태로 만든다. 클린 아키텍쳐의 주요한 목표는 개발자들의 비지니스로직을 캡슐화 할수있게 조직적인 코드를 만들고 캡슐화된 비지니스 로직을 delivery mechianism로 부터 분단하는 것이다.

=> 내생각엔 그냥 delivery mechanism은 유저와 맡닿아 있는 interface같고 그 interface와 비지니스로직을 분단시키고 비지니스로직을 캡슐화하여 재사용성을 높여 코드의 재사용성을 높혀 생산성을 높히고 개발에 용이하도록 하는것이 클린 아키텍쳐라는 뜻같음

About the framework, I liked the possibility of using a framework made for typescript and the documentation is really completed. But after my first project with this framework, I realized that the common architecture from the documentation was not the best choice. Of course, at the beginning everything is fine, the problem arrive always after some months when the specs change again and again. For that reason, I tried a few architectures and stoped on clean architecture. Clean architecture gives me a solution to my problem.

- 프레임 워크에 관해서는, 타입스크립트를 제공하고 docs자체 만으로도 너무 좋다고 생각했다. 그런데 첫번째 프로젝트 경험후 nestjs에서 제공하는 docs에 관한 아키텍처 만으로는 완벽하지 않다고 느꼈다. 항상 그렇듯 처음에는 괜찮은데 몇달지나 계속된 수정이 이루어질때쯤 되면 문제를 발견할수있다. 그래서 몇가지 아키텍팅 방법을 시도해봤고 clean architecture에서 정착하여 답을 얻을수있었다.

-   How to make a code robust and easy to maintain.
-   How to dispatch the work in a team.
-   How to work even if you don’t have all spec, database, etc…

- 어떻하면 튼튼하고 쉬운 유지보수를 할수있을까
- 어떻게 하면 협업에 용이할까
- 모든 spec에 대한 정보와 database등의 정보없이 어떻게 작업할수있을까?


![[Pasted image 20220901152246.png]]
-   domain contains the business code and its logic and has no outward dependency: nor on frameworks (NestJS in our case), nor on use_cases or infrastructure packages.
-   usecases is like a conductor. It will depend only on domain package to execute business logic. use_cases should not have any dependencies on infrastructure (including framework or npm module).
-   infrastructure contains all the technical details, configuration, implementations (database, web services, npm module, etc.), and must not contain any business logic. infrastructure has dependencies on domain, use_cases and frameworks.

- domain: 비지니스 로직만 담는다. 유즈케이스나 인프라에 관한 내용은 담지않는다.
- useCases: useCases는 지휘자와 같다. 이것은 도메인으로 부터만 의존성을 주입받는다. useCases는 다른곳에서 비지니스로직을 의존성주입 받아서도 안돼고 infra로 부터 받아서도 안됀다.
- infrastructure: infra는 기술적인 디테일, 설정, 실행사항(database, web services, npm module등) 그리고 infra에는 비지니스 로직이 담겨서는 절대 안된다. infra는 domain과 useCases로 부터 의존성을 주입받을수있다.