<p align="center">
    <img width="380" src="https://github.com/738/clean-code-typescript/blob/master/clean-code-typescript.png?raw=true" />
</p>
<h1 align="center">
    clean-code-typescript
</h1>

<p align="center">
  <a href="https://twitter.com/intent/tweet?text=클린코드%20타입스크립트&url=https://github.com/738/clean-code-typescript">
      <img src="https://img.shields.io/twitter/url/http/shields.io.svg?style=social" alt="Tweet">
  </a>
  <a href="https://hits.seeyoufarm.com">
      <img src="https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2F738%2Fclean-code-typescript" alt="Tweet">
  </a>
</p>

<p align="center">
  <b>타입스크립트를 위한 클린코드</b></br>
  <a href="https://github.com/ryanmcdermott/clean-code-javascript">clean-code-javascript</a>에서 영감을 받았습니다.
</p>

## 목차

  1. [소개](#소개)
  2. [변수](#변수)
  3. [함수](#함수)
  4. [객체와 자료구조](#객체와-자료구조)
  5. [클래스](#클래스)
  6. [SOLID](#solid)
  7. [테스트](#테스트)
  8. [동시성](#동시성)
  9. [에러 처리](#에러-처리)
  10. [서식](#서식)
  11. [주석](#주석)
  12. [번역](#번역)
  13. [번역에 도움을 주신 분들](#번역에-도움을-주신-분들)

## 소개

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

Robert C. Martin의 책인 [*클린 코드*](http://www.yes24.com/Product/Goods/11681152)에 있는 소프트웨어 공학 방법론을 타입스크립트에 적용한 글입니다. 이 글은 스타일 가이드가 아닙니다. 이 글은 타입스크립트에서 [읽기 쉽고, 재사용 가능하며, 리팩토링 가능한](https://github.com/ryanmcdermott/3rs-of-software-architecture) 소프트웨어를 작성하기 위한 가이드입니다.

여기 있는 모든 규칙을 엄격하게 따를 필요는 없으며, 보편적으로 통용되는 규칙은 아닙니다. 이 글은 하나의 지침일 뿐이며, *클린 코드*의 저자가 수년간 경험한 내용을 바탕으로 정리한 것입니다.

소프트웨어 공학 기술의 역사는 50년이 조금 넘었고, 배워야 할 것이 여전히 많습니다. 소프트웨어 설계가 건축 설계만큼 오래되었을 때는 아마도 아래 규칙들보다 엄격한 규칙을 따라야 할 것입니다. 하지만 지금은 이 지침을 당신과 당신 팀이 작성하는 타입스크립트 코드의 품질을 평가하는 기준으로 삼으세요.

한 가지 더 말씀드리자면, 이 규칙들을 알게 된다 해서 당장 더 나은 개발자가 되는 것은 아니며 코드를 작성할 때 실수를 하지 않게 해주는 것은 아닙니다. 젖은 점토가 최종의 결과물로 빚어지는 것처럼 모든 코드들도 처음 작성한 코드로 시작됩니다. 결국은 동료들과 리뷰하면서 결점이 제거됩니다. 당신이 처음 작성한 코드에 개선이 필요할 때 자책하지 마세요. 대신 코드가 더 나아지도록 두들기세요!

**[⬆ 맨 위로 이동](#목차)**

## 변수

### 의미있는 변수 이름을 사용하세요

읽는 사람으로 하여금 변수마다 어떤 점이 다른지 알 수 있도록 이름을 구별하세요.

**Bad:**

```ts
function between<T>(a1: T, a2: T, a3: T): boolean {
  return a2 <= a1 && a1 <= a3;
}

```

**Good:**

```ts
function between<T>(value: T, left: T, right: T): boolean {
  return left <= value && value <= right;
}
```

**[⬆ 맨 위로 이동](#목차)**

### 발음할 수 있는 변수 이름을 사용하세요

발음할 수 없는 이름은 그 변수에 대해서 바보 같이 소리를 내 토론할 수밖에 없습니다.

**Bad:**

```ts
type DtaRcrd102 = {
  genymdhms: Date;
  modymdhms: Date;
  pszqint: number;
}
```

**Good:**

```ts
type Customer = {
  generationTimestamp: Date;
  modificationTimestamp: Date;
  recordId: number;
}
```

**[⬆ 맨 위로 이동](#목차)**

### 동일한 유형의 변수는 동일한 단어를 사용하세요

**Bad:**

```ts
function getUserInfo(): User;
function getUserDetails(): User;
function getUserData(): User;
```

**Good:**

```ts
function getUser(): User;
```

**[⬆ 맨 위로 이동](#목차)**

### 검색할 수 있는 이름을 사용하세요

코드를 쓸 때보다 읽을 때가 더 많기 때문에 우리가 쓰는 코드는 읽을 수 있고 검색이 가능해야 합니다. 프로그램을 이해할 때 의미있는 변수 이름을 짓지 않으면 읽는 사람으로 하여금 어려움을 줄 수 있습니다. 검색 가능한 이름을 지으세요. [TSLint](https://palantir.github.io/tslint/rules/no-magic-numbers/)와 같은 도구는 이름이 없는 상수를 식별할 수 있도록 도와줍니다.

**Bad:**

```ts
// 86400000이 도대체 뭐지?
setTimeout(restart, 86400000);
```

**Good:**

```ts
// 대문자로 이루어진 상수로 선언하세요.
const MILLISECONDS_IN_A_DAY = 24 * 60 * 60 * 1000;

setTimeout(restart, MILLISECONDS_IN_A_DAY);
```

**[⬆ 맨 위로 이동](#목차)**

### 의도를 나타내는 변수를 사용하세요

**Bad:**

```ts
declare const users: Map<string, User>;

for (const keyValue of users) {
  // users 맵을 순회
}
```

**Good:**

```ts
declare const users: Map<string, User>;

for (const [id, user] of users) {
  // users 맵을 순회
}
```

**[⬆ 맨 위로 이동](#목차)**

### 암시하는 이름은 사용하지 마세요

명시적인 것이 암시적인 것보다 좋습니다.  
*명료함은 최고입니다.*

**Bad:**

```ts
const u = getUser();
const s = getSubscription();
const t = charge(u, s);
```

**Good:**

```ts
const user = getUser();
const subscription = getSubscription();
const transaction = charge(user, subscription);
```

**[⬆ 맨 위로 이동](#목차)**

### 불필요한 문맥은 추가하지 마세요

클래스/타입/객체의 이름에 의미가 담겨있다면, 변수 이름에서 반복하지 마세요.

**Bad:**

```ts
type Car = {
  carMake: string;
  carModel: string;
  carColor: string;
}

function print(car: Car): void {
  console.log(`${car.carMake} ${car.carModel} (${car.carColor})`);
}
```

**Good:**

```ts
type Car = {
  make: string;
  model: string;
  color: string;
}

function print(car: Car): void {
  console.log(`${car.make} ${car.model} (${car.color})`);
}
```

**[⬆ 맨 위로 이동](#목차)**

### short circuiting이나 조건문 대신 기본 매개변수를 사용하세요

기본 매개변수는 short circuiting보다 보통 명료합니다.

**Bad:**

```ts
function loadPages(count?: number) {
  const loadCount = count !== undefined ? count : 10;
  // ...
}
```

**Good:**

```ts
function loadPages(count: number = 10) {
  // ...
}
```

**[⬆ 맨 위로 이동](#목차)**

### 의도를 알려주기 위해 `enum`을 사용하세요

예를 들어 그것들의 값 자체보다 값이 구별되어야 할 때와 같이 코드의 의도를 알려주는데에 `enum`은 도움을 줄 수 있습니다.

**Bad:**

```ts
const GENRE = {
  ROMANTIC: 'romantic',
  DRAMA: 'drama',
  COMEDY: 'comedy',
  DOCUMENTARY: 'documentary',
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // Projector의 선언
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
        // 실행되어야 하는 로직
    }
  }
}
```

**Good:**

```ts
enum GENRE {
  ROMANTIC,
  DRAMA,
  COMEDY,
  DOCUMENTARY,
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // Projector의 선언
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
        // 실행되어야 하는 로직
    }
  }
}
```

**[⬆ 맨 위로 이동](#목차)**

## 함수

### 함수의 매개변수는 2개 혹은 그 이하가 이상적입니다

함수 매개변수의 개수를 제한하는 것은 함수를 테스트하기 쉽게 만들어주기 때문에 놀라울 정도로 중요합니다.
함수 매개변수가 3개 이상인 경우, 각기 다른 인수로 여러 다른 케이스를 테스트해야 하므로 경우의 수가 매우 많아집니다.  

한 개 혹은 두 개의 매개변수가 이상적인 경우고, 가능하다면 세 개는 피해야 합니다. 그 이상의 경우에는 합쳐야 합니다.
두 개 이상의 매개변수를 가질 경우, 함수가 많은 것을 할 가능성이 높아집니다.
그렇지 않은 경우, 대부분 상위 객체는 하나의 매개변수로 충분할 것입니다.   

많은 매개변수를 사용해야 한다면 객체 리터럴을 사용하는 것을 고려해보세요.  

함수가 기대하는 속성을 명확하게 하기 위해, [구조 분해](https://basarat.gitbook.io/typescript/future-javascript/destructuring) 구문을 사용할 수 있습니다.
이 구문은 몇 개의 장점을 가지고 있습니다:  

1. 어떤 사람이 함수 시그니쳐(매개변수의 타입, 반환값의 타입 등)를 볼 때, 어떤 속성이 사용되는지 즉시 알 수 있습니다.

2. 명명된 매개변수처럼 보이게 할 때 사용할 수 있습니다.

3. 또한 구조 분해는 함수로 전달된 매개변수 객체의 특정한 원시 값을 복제하며 이것은 사이드 이펙트를 방지하는데 도움을 줍니다. 유의사항: 매개변수 객체로부터 구조 분해된 객체와 배열은 **복제되지 않습니다.**

4. 타입스크립트는 사용하지 않은 속성에 대해서 경고를 주며, 구조 분해를 사용하면 경고를 받지 않을 수 있습니다.

**Bad:**

```ts
function createMenu(title: string, body: string, buttonText: string, cancellable: boolean) {
  // ...
}

createMenu('Foo', 'Bar', 'Baz', true);
```

**Good:**

```ts
function createMenu(options: { title: string, body: string, buttonText: string, cancellable: boolean }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

[타입 앨리어스](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases)를 사용해서 가독성을 더 높일 수 있습니다:

```ts

type MenuOptions = { title: string, body: string, buttonText: string, cancellable: boolean };

function createMenu(options: MenuOptions) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

**[⬆ 맨 위로 이동](#목차)**

### 함수는 한 가지만 해야합니다

이것은 소프트웨어 공학에서 단연코 가장 중요한 규칙입니다. 함수가 한 가지 이상의 역할을 수행할 때 작성하고 테스트하고 추론하기 어려워집니다. 함수를 하나의 행동으로 정의할 수 있을 때, 쉽게 리팩토링할 수 있으며 코드를 더욱 명료하게 읽을 수 있습니다.
이 가이드에서 이 부분만 자기것으로 만들어도 당신은 많은 개발자보다 앞설 수 있습니다.

**Bad:**

```ts
function emailClients(clients: Client[]) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good:**

```ts
function emailClients(clients: Client[]) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client: Client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ 맨 위로 이동](#목차)**

### 함수가 무엇을 하는지 알 수 있도록 함수 이름을 지으세요

**Bad:**

```ts
function addToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();

// 무엇이 추가되는지 함수 이름만으로 유추하기 어렵습니다
addToDate(date, 1);
```

**Good:**

```ts
function addMonthToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();
addMonthToDate(date, 1);
```

**[⬆ 맨 위로 이동](#목차)**

### 함수는 단일 행동을 추상화해야 합니다

함수가 한 가지 이상을 추상화한다면 그 함수는 너무 많은 일을 하게 됩니다. 재사용성과 쉬운 테스트를 위해서 함수를 쪼개세요.

**Bad:**

```ts
function parseCode(code: string) {
  const REGEXES = [ /* ... */ ];
  const statements = code.split(' ');
  const tokens = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**Good:**

```ts
const REGEXES = [ /* ... */ ];

function parseCode(code: string) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);

  syntaxTree.forEach((node) => {
    // parse...
  });
}

function tokenize(code: string): Token[] {
  const statements = code.split(' ');
  const tokens: Token[] = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      tokens.push( /* ... */ );
    });
  });

  return tokens;
}

function parse(tokens: Token[]): SyntaxTree {
  const syntaxTree: SyntaxTree[] = [];
  tokens.forEach((token) => {
    syntaxTree.push( /* ... */ );
  });

  return syntaxTree;
}
```

**[⬆ 맨 위로 이동](#목차)**

### 중복된 코드를 제거해주세요

코드가 중복되지 않도록 최선을 다하세요.
중복된 코드는 어떤 로직을 변경할 때 한 곳 이상을 변경해야 하기 때문에 좋지 않습니다.   

당신이 레스토랑을 운영하면서 재고를 추적한다고 상상해보세요: 모든 토마토, 양파, 마늘, 양념 등.
관리하는 목록이 여러개일 때 토마토를 넣은 요리를 제공할 때마다 모든 목록을 수정해야 합니다.
관리하는 목록이 단 하나일 때는 한 곳만 수정하면 됩니다! 

당신은 종종 두 개 이상의 사소한 차이점이 존재한다고 생각해서 거의 비슷한 코드를 중복 작성합니다. 하지만 그 몇가지 다른 것으로 인해 같은 역할을 하는 두 개 이상의 함수를 만들게 됩니다. 중복된 코드를 제거하는 것은 조금씩 다른 역할을 하는 것을 묶음으로써 하나의 함수/모듈/클래스로 처리하는 추상화를 만드는 것을 의미합니다.

추상화를 올바르게 하는 것은 중요하며, 이것은 [SOLID](#solid) 원칙을 따르는 이유이기도 합니다. 올바르지 않은 추상화는 중복된 코드보다 나쁘므로 주의하세요! 좋은 추상화를 할 수 있다면 그렇게 하라는 말입니다! 반복하지 마세요. 그렇지 않으면 하나를 변경할 때마다 여러 곳을 변경하게 될 것입니다.

**Bad:**

```ts
function showDeveloperList(developers: Developer[]) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();

    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers: Manager[]) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Good:**

```ts
class Developer {
  // ...
  getExtraDetails() {
    return {
      githubLink: this.githubLink,
    }
  }
}

class Manager {
  // ...
  getExtraDetails() {
    return {
      portfolio: this.portfolio,
    }
  }
}

function showEmployeeList(employee: Developer | Manager) {
  employee.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const extra = employee.getExtraDetails();

    const data = {
      expectedSalary,
      experience,
      extra,
    };

    render(data);
  });
}
```

당신은 중복된 코드에 대해서 비판적으로 생각해야 합니다. 가끔은 중복된 코드와 불필요한 추상화로 인한 복잡성 간의 맞바꿈이 있을 수 있습니다. 서로 다른 두 개의 모듈의 구현이 유사해 보이지만 서로 다른 도메인에 존재하는 경우, 코드 중복은 공통된 코드에서 추출해서 중복을 줄이는 것보다 나은 선택일 수 있습니다. 이 경우에 추출된 공통의 코드는 두 모듈 사이에서 간접적인 의존성이 나타나게 됩니다.

**[⬆ 맨 위로 이동](#목차)**

### `Object.assign` 혹은 구조 분해를 사용해서 기본 객체를 만드세요

**Bad:**

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu(config: MenuConfig) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable !== undefined ? config.cancellable : true;

  // ...
}

createMenu({ body: 'Bar' });
```

**Good:**

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu(config: MenuConfig) {
  const menuConfig = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // ...
}

createMenu({ body: 'Bar' });
```

대안으로, 기본 값을 구조 분해를 사용해서 해결할 수 있습니다:

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu({ title = 'Foo', body = 'Bar', buttonText = 'Baz', cancellable = true }: MenuConfig) {
  // ...
}

createMenu({ body: 'Bar' });
```

사이드 이펙트와 `undefined` 혹은 `null` 값을 명시적으로 넘기는 예상치 못한 행동을 피하기 위해서 타입스크립트 컴파일러에게 그것을 허락하지 않도록 설정할 수 있습니다. 타입스크립트에서 [`--strictNullChecks`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#--strictnullchecks) 옵션을 확인하세요.

**[⬆ 맨 위로 이동](#목차)**

### 함수 매개변수로 플래그를 사용하지 마세요

플래그를 사용하는 것은 해당 함수가 한 가지 이상의 일을 한다는 것을 뜻합니다.
함수는 한 가지의 일을 해야합니다. boolean 변수로 인해 다른 코드가 실행된다면 그 함수를 쪼개도록 하세요.

**Bad:**

```ts
function createFile(name: string, temp: boolean) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Good:**

```ts
function createTempFile(name: string) {
  createFile(`./temp/${name}`);
}

function createFile(name: string) {
  fs.create(name);
}
```

**[⬆ 맨 위로 이동](#목차)**

### 사이드 이펙트를 피하세요 (파트 1)

함수는 값을 가져와서 다른 값을 반환하는 것 이외에 다른 것을 할 경우 사이드 이펙트를 발생시킬 수 있습니다. 사이드 이펙트는 파일을 쓴다거나, 전역 변수를 조작한다거나, 뜻하지 않게 낯선 사람에게 당신의 전재산을 송금할 수 있습니다.

당신은 가끔 프로그램에서 사이드 이펙트를 가질 필요가 있습니다. 이전의 사례에서와 같이 당신은 파일을 써야할 때가 있습니다.
당신이 하고 싶은 것은 이것을 중앙화하는 것입니다. 특정 파일을 쓰기 위해 몇 개의 함수와 클래스를 만들지 마세요.
그것을 행하는 서비스를 단 하나만 만드세요.

중요한 것은 어떠한 구조도 없이 객체 사이에 상태를 공유하거나 어떤 것에 의해서든지 변경될 수 있는 데이터 타입을 사용하거나 사이드 이펙트가 일어나는 곳을 중앙화 하지 않는 것과 같은 위험 요소를 피하는 것입니다. 만약 그렇게 할 수 있다면, 당신은 대부분의 다른 프로그래머들보다 더욱 행복할 것입니다.

**Bad:**

```ts
// 아래의 함수에서 참조하는 전역 변수입니다.
let name = 'Robert C. Martin';

function toBase64() {
  name = btoa(name);
}

toBase64();
// 이 이름을 사용하는 다른 함수가 있다면, 그것은 Base64 값을 반환할 것입니다

console.log(name); // 'Robert C. Martin'이 출력되는 것을 예상했지만 'Um9iZXJ0IEMuIE1hcnRpbg=='가 출력됨
```

**Good:**

```ts
const name = 'Robert C. Martin';

function toBase64(text: string): string {
  return btoa(text);
}

const encodedName = toBase64(name);
console.log(name);
```

**[⬆ 맨 위로 이동](#목차)**

### 사이드 이펙트를 피하세요 (파트 2)

자바스크립트에서 원시값은 값에 의해 전달되고 객체/배열은 참조에 의해 전달됩니다. 예를 들어, 객체와 배열의 경우 어떤 함수가 쇼핑 장바구니 배열을 변경하는 기능을 가지고 있다면, 구매하려는 아이템이 추가됨으로써 `cart` 배열을 사용하는 다른 함수는 이 추가의 영향을 받을 수 있습니다. 이것은 장점이 될 수도 있지만 단점이 될 수도 있습니다. 최악의 상황을 상상해보겠습니다:  

사용자는 네트워크 요청을 생성하고 서버에 `cart` 배열을 전송하는 `purchase` 함수를 호출하는 "구매" 버튼을 클릭합니다. 네트워크 연결 불량 때문에 `purchase` 함수는 요청을 재시도해야 합니다. 네트워크 요청이 시작되기 전에 사용자가 원하지 않은 아이템을 실수로 "장바구니에 추가하기" 버튼을 누르면 어떻게 될까요? 네트워크 요청이 시작되면, `purchase` 함수는 `addItemToCart` 함수가 변경한 쇼핑 장바구니 배열을 참조하고 있기 때문에 `purchase` 함수는 실수로 추가된 아이템을 보낼 것입니다.  

훌륭한 해법은 `addItemToCart` 함수에서 `cart` 배열을 복제하고 그것을 수정하고 그 복제한 값을 반환하는 것입니다. 이는 쇼핑 장바구니 배열을 참조하고 있는 값을 들고 있는 어떤 다른 함수도 다른 변경에 의해 영향을 받지 않는 것을 보장합니다.  

이 접근법에 대한 두 가지 주의사항:

1. 실제로는 입력된 객체값을 변경하기를 원하는 경우가 있을 수 있습니다. 하지만 이러한 프로그래밍 관례를 선택할 때 당신은 이러한 경우가 매우 드물다는 것을 알게 될 것입니다. 대부분은 사이드 이펙트가 없도록 리팩토링될 수 있습니다! ([순수 함수](https://en.wikipedia.org/wiki/Pure_function)를 확인해주세요)

3. 큰 객체를 복제하는 것은 성능 관점에서 비용이 높을 수 있습니다. 다행히도 이러한 프로그래밍 접근법을 가능하게 해주는 훌륭한 라이브러리가 있기 때문에 큰 문제는 아닙니다. 이는 수동으로 객체와 배열을 복제해주는 것만큼 메모리 집약적이지 않게 해주고 빠르게 복제해줍니다.

**Bad:**

```ts
function addItemToCart(cart: CartItem[], item: Item): void {
  cart.push({ item, date: Date.now() });
};
```

**Good:**

```ts
function addItemToCart(cart: CartItem[], item: Item): CartItem[] {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ 맨 위로 이동](#목차)**

### 전역 함수를 작성하지 마세요

전역을 더럽히는 것은 자바스크립트에서 나쁜 관습입니다. 왜냐하면 다른 라이브러리와 충돌날 수 있고 당신의 API의 사용자는 상용에서 예외가 발생할 때까지 전혀 모를 것이기 때문입니다. 한 예제를 생각해보겠습니다: 당신이 자바스크립트 네이티브 배열 메소드를 확장해서 두 배열 사이의 다른 점을 보여주는 `diff` 메소드를 추가하고 싶다면 어떨까요? `Array.prototype`에 당신의 새로운 함수를 작성할 것입니다. 하지만 동일한 기능을 수행하고 있는 다른 라이브러리와 충돌날 수 있습니다. 다른 라이브러리에서는 배열에서 첫 번째 요소와 마지막 요소 사이의 다름만 찾기 위해 `diff` 함수를 사용한다면 어떨까요? 이것이 단지 클래스를 사용해서 전역 `Array`를 상속하는 것이 더 좋은 이유입니다.

**Bad:**

```ts
declare global {
  interface Array<T> {
    diff(other: T[]): Array<T>;
  }
}

if (!Array.prototype.diff) {
  Array.prototype.diff = function <T>(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter(elem => !hash.has(elem));
  };
}
```

**Good:**

```ts
class MyArray<T> extends Array<T> {
  diff(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter(elem => !hash.has(elem));
  };
}
```

**[⬆ 맨 위로 이동](#목차)**

### 명령형 프로그래밍보다 함수형 프로그래밍을 지향하세요

가능하다면 이런 방식의 프로그래밍을 지향하세요.

**Bad:**

```ts
const contributions = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < contributions.length; i++) {
  totalOutput += contributions[i].linesOfCode;
}
```

**Good:**

```ts
const contributions = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

const totalOutput = contributions
  .reduce((totalLines, output) => totalLines + output.linesOfCode, 0);
```

**[⬆ 맨 위로 이동](#목차)**

### 조건문을 캡슐화하세요

**Bad:**

```ts
if (subscription.isTrial || account.balance > 0) {
  // ...
}
```

**Good:**

```ts
function canActivateService(subscription: Subscription, account: Account) {
  return subscription.isTrial || account.balance > 0;
}

if (canActivateService(subscription, account)) {
  // ...
}
```

**[⬆ 맨 위로 이동](#목차)**

### 부정 조건문을 피하세요

**Bad:**

```ts
function isEmailNotUsed(email: string): boolean {
  // ...
}

if (isEmailNotUsed(email)) {
  // ...
}
```

**Good:**

```ts
function isEmailUsed(email): boolean {
  // ...
}

if (!isEmailUsed(node)) {
  // ...
}
```

**[⬆ 맨 위로 이동](#목차)**

### 조건문을 피하세요

불가능해보일 수 있습니다. 처음 이를 본 대부분의 사람들은 "대체 `if`문 없이 뭘 할 수 있나요?" 라고 반응합니다. 하지만 많은 경우에 다형성을 사용한다면 해결할 수 있습니다. 그 다음 반응으로는 "좋아요. 하지만 왜 그래야하죠?" 입니다. 이에 대한 해답은 우리가 이전에 배운 클린 코드 컨셉 중 "함수는 한 가지 일만 해야합니다" 입니다. `if`문이 있는 클래스와 함수가 있다면, 그 함수는 한 가지 이상의 일을 하고 있다는 것입니다. 함수는 한 가지 일만 해야한다는 것을 기억하세요.

**Bad:**

```ts
class Airplane {
  private type: string;
  // ...

  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return this.getMaxAltitude() - this.getPassengerCount();
      case 'Air Force One':
        return this.getMaxAltitude();
      case 'Cessna':
        return this.getMaxAltitude() - this.getFuelExpenditure();
      default:
        throw new Error('Unknown airplane type.');
    }
  }

  private getMaxAltitude(): number {
    // ...
  }
}
```

**Good:**

```ts
abstract class Airplane {
  protected getMaxAltitude(): number {
    // shared logic with subclasses ...
  }

  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ 맨 위로 이동](#목차)**

### 타입 체킹을 피하세요

타입스크립트는 자바스크립트의 엄격한 구문적 상위 집합이며 언어에 선택적인 정적 타입 검사 기능을 추가합니다.
타입스크립트의 기능을 최대한 활용하기 위해 항상 변수의 타입, 매개변수, 반환값의 타입을 지정하도록 하세요.
그렇게 하면 리팩토링이 매우 쉬워집니다.

**Bad:**

```ts
function travelToTexas(vehicle: Bicycle | Car) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(currentLocation, new Location('texas'));
  }
}
```

**Good:**

```ts
type Vehicle = Bicycle | Car;

function travelToTexas(vehicle: Vehicle) {
  vehicle.move(currentLocation, new Location('texas'));
}
```

**[⬆ 맨 위로 이동](#목차)**

### 필요 이상으로 최적화하지 마세요

현대 브라우저는 런타임에서 많은 최적화를 수행합니다. 많은 시간을 최적화하는 데에 사용하고 있다면 시간 낭비입니다. 최적화가 부족한 부분을 확인할 수 있는 좋은 [자료](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)가 있습니다. 이것을 참조하여 최적화가 부족한 부분만 최적화해줄 수 있습니다.

**Bad:**

```ts
// 예전 브라우저에서는 캐시되지 않은 `list.length`를 사용한 각 순회는 비용이 많이 들 것입니다.
// `list.length`의 재계산 때문입니다. 현대 브라우저에서는 이 부분이 최적화됩니다.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Good:**

```ts
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ 맨 위로 이동](#목차)**

### 필요하지 않는 코드는 제거하세요

사용하지 않은 코드는 중복된 코드만큼 나쁩니다. 당신의 코드에서 이것을 유지할 이유는 없습니다.
호출되지 않은 코드가 있다면 제거하세요! 지운 코드를 다시 확인할 필요가 있다면 버전 기록에서 볼 수 있습니다.

**Bad:**

```ts
function oldRequestModule(url: string) {
  // ...
}

function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**Good:**

```ts
function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**[⬆ 맨 위로 이동](#목차)**

### `iterator`와 `generator`를 사용하세요

스트림과 같이 사용되는 데이터 콜렉션을 사용할 때는 `generator`와 `iterable`을 사용하세요.
몇 가지의 좋은 이유가 있습니다:

- 피호출자가 접근할 아이템 수를 결정한다는 의미에서 피호출자를 `generator` 구현으로부터 분리할 수 있습니다.
- 지연 실행, 아이템은 요구에 의해 스트림 처리될 수 있습니다.
- `for-of` 구문을 사용해 아이템을 순회하는 내장 지원이 있습니다.
- `iterable`은 최적화된 `iterator` 패턴을 구현할 수 있습니다.

**Bad:**

```ts
function fibonacci(n: number): number[] {
  if (n === 1) return [0];
  if (n === 2) return [0, 1];

  const items: number[] = [0, 1];
  while (items.length < n) {
    items.push(items[items.length - 2] + items[items.length - 1]);
  }

  return items;
}

function print(n: number) {
  fibonacci(n).forEach(fib => console.log(fib));
}

// 피보나치 숫자의 첫 번째 10개 숫자를 출력합니다.
print(10);
```

**Good:**

```ts
// 피보나치 숫자의 무한 스트림을 생성합니다.
// `generator`는 모든 숫자의 배열을 유지하고 있지 않습니다.
function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

function print(n: number) {
  let i = 0;
  for (const fib of fibonacci()) {
    if (i++ === n) break;  
    console.log(fib);
  }  
}

// 피보나치 숫자의 첫 번째 10개 숫자를 출력합니다.
print(10);
```

`map`, `slice`, `forEach` 등과 같은 메소드를 연결함으로써 네이티브 배열을 비슷한 방법으로 `iterable`로 작업할 수 있게 하는 라이브러리가 있습니다.
`iterable`의 발전된 조작의 사례를 위해 [itiriri](https://www.npmjs.com/package/itiriri)를 확인해주세요. (또는 비동기 `iterable`의 조작을 위해서 [itiriri-async](https://www.npmjs.com/package/itiriri-async)를 확인해주세요.)

```ts
import itiriri from 'itiriri';

function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];
 
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

itiriri(fibonacci())
  .take(10)
  .forEach(fib => console.log(fib));
```

**[⬆ 맨 위로 이동](#목차)**

## 객체와 자료구조

### `getter`와 `setter`를 사용하세요

타입스크립트는 `getter`/`setter` 구문을 지원합니다.
행동을 캡슐화한 객체에서 데이터를 접근하기 위해 `getter`와 `setter`를 사용하는 것은 객체에서 속성을 단순히 찾는 것보다 낫습니다.
"왜 그렇습니까?" 라고 물을 수 있습니다. 다음과 같은 이유가 있습니다:

- 객체 속성을 얻는 것 이상으로 무언가를 더 하고 싶을 때, 코드 안에서 관련된 모든 접근자를 찾고 변경하지 않아도 됩니다.
- `set`을 사용할 때 검증 로직을 추가하는 것이 간단합니다.
- 내부의 API를 캡슐화할 수 있습니다.
- 값을 조회하고 설정할 때 로그를 기록하고 에러를 처리하는 것이 쉽습니다.
- 서버에서 객체 속성을 불러올 때 지연 로딩할 수 있습니다.

**Bad:**

```ts
type BankAccount = {
  balance: number;
  // ...
}

const value = 100;
const account: BankAccount = {
  balance: 0,
  // ...
};

if (value < 0) {
  throw new Error('Cannot set negative balance.');
}

account.balance = value;
```

**Good:**

```ts
class BankAccount {
  private accountBalance: number = 0;

  get balance(): number {
    return this.accountBalance;
  }

  set balance(value: number) {
    if (value < 0) {
      throw new Error('Cannot set negative balance.');
    }

    this.accountBalance = value;
  }

  // ...
}

// 이제 `BankAccount`는 검증 로직을 캡슐화합니다.
// 명세가 바뀐다면, 추가적인 검증 규칙을 추가할 필요가 있습니다.
// 그 때, `setter` 구현부만 수정하면 됩니다.
// 관련있는 다른 코드는 변경할 필요가 없습니다.
const account = new BankAccount();
account.balance = 100;
```

**[⬆ 맨 위로 이동](#목차)**

### private/protected 멤버를 갖는 객체를 생성하세요

타입스크립트는 클래스 멤버를 위해 `public` *기본*, `protected`, `private` 접근자를 지원합니다.

**Bad:**

```ts
class Circle {
  radius: number;
  
  constructor(radius: number) {
    this.radius = radius;
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**Good:**

```ts
class Circle {
  constructor(private readonly radius: number) {
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**[⬆ 맨 위로 이동](#목차)**

### 불변성을 선호하세요

타입스크립트의 타입 시스템은 `interface`/`class`의 개별 속성을 *readonly*로 표시할 수 있습니다. 이를 통해 기능적인 방식으로 작업할 수 있습니다. (예상하지 않은 변조는 위험합니다.)  
더욱 나은 방법으로는 타입 `T`를 갖고 mapped types를 사용하여 모든 각 속성을 읽기 전용으로 표시하는 `Readonly` 내장 타입이 존재합니다. ([mapped types](https://www.typescriptlang.org/docs/handbook/advanced-types.html#mapped-types)를 확인하세요.)

**Bad:**

```ts
interface Config {
  host: string;
  port: string;
  db: string;
}
```

**Good:**

```ts
interface Config {
  readonly host: string;
  readonly port: string;
  readonly db: string;
}
```

배열의 경우, `ReadonlyArray<T>`를 사용해서 읽기 전용의 배열을 생성할 수 있습니다.
이것은 `push()`와 `fill()`과 같은 변경을 막습니다. 하지만 값 자체를 변경하지 않는 `concat()`, `slice()`과 같은 기능은 사용할 수 있습니다.

**Bad:**

```ts
const array: number[] = [ 1, 3, 5 ];
array = []; // 에러
array.push(100); // 배열은 변경될 것입니다.
```

**Good:**

```ts
const array: ReadonlyArray<number> = [ 1, 3, 5 ];
array = []; // 에러
array.push(100); // 에러
```

[TypeScript 3.4 is a bit easier](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#improvements-for-readonlyarray-and-readonly-tuples)에서 읽기 전용의 매개변수를 선언할 수 있습니다.

```ts
function hoge(args: readonly string[]) {
  args.push(1); // 에러
}
```

리터럴 값을 위해 [const assertions](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#const-assertions)를 사용하세요.

**Bad:**

```ts
const config = {
  hello: 'world'
};
config.hello = 'world'; // 값이 바뀝니다

const array  = [ 1, 3, 5 ];
array[0] = 10; // 값이 바뀝니다

// 쓸 수 있는 객체가 반환됩니다
function readonlyData(value: number) {
  return { value };
}

const result = readonlyData(100);
result.value = 200; // 값이 바뀝니다
```

**Good:**

```ts
// 읽기 전용 객체
const config = {
  hello: 'world'
} as const;
config.hello = 'world'; // 에러

// 읽기 전용 배열
const array  = [ 1, 3, 5 ] as const;
array[0] = 10; // 에러

// 읽기 전용 객체를 반활할 수 있습니다
function readonlyData(value: number) {
  return { value } as const;
}

const result = readonlyData(100);
result.value = 200; // 에러
```

**[⬆ 맨 위로 이동](#목차)**

### 타입 vs 인터페이스

합집합 또는 교집합이 필요할 때 타입을 사용하세요. `extends` 또는 `implements`가 필요할 때 인터페이스를 사용하세요. 엄격한 규칙은 없지만 당신에게 맞는 방법을 사용하세요.  
타입스크립트에서 `type`과 `interface` 사이의 다른 점에 대해서 더 상세한 설명을 원한다면 이 [답변](https://stackoverflow.com/questions/37233735/typescript-interfaces-vs-types/54101543#54101543)을 참고하세요.

**Bad:**

```ts
interface EmailConfig {
  // ...
}

interface DbConfig {
  // ...
}

interface Config {
  // ...
}

//...

type Shape = {
  // ...
}
```

**Good:**

```ts

type EmailConfig = {
  // ...
}

type DbConfig = {
  // ...
}

type Config  = EmailConfig | DbConfig;

// ...

interface Shape {
  // ...
}

class Circle implements Shape {
  // ...
}

class Square implements Shape {
  // ...
}
```

**[⬆ 맨 위로 이동](#목차)**

## 클래스

### 클래스는 작아야 합니다

클래스의 크기는 책임에 의해 측정됩니다. *단일 책임 원칙*에 따르면 클래스는 작아야 합니다.

**Bad:**

```ts
class Dashboard {
  getLanguage(): string { /* ... */ }
  setLanguage(language: string): void { /* ... */ }
  showProgress(): void { /* ... */ }
  hideProgress(): void { /* ... */ }
  isDirty(): boolean { /* ... */ }
  disable(): void { /* ... */ }
  enable(): void { /* ... */ }
  addSubscription(subscription: Subscription): void { /* ... */ }
  removeSubscription(subscription: Subscription): void { /* ... */ }
  addUser(user: User): void { /* ... */ }
  removeUser(user: User): void { /* ... */ }
  goToHomePage(): void { /* ... */ }
  updateProfile(details: UserDetails): void { /* ... */ }
  getVersion(): string { /* ... */ }
  // ...
}

```

**Good:**

```ts
class Dashboard {
  disable(): void { /* ... */ }
  enable(): void { /* ... */ }
  getVersion(): string { /* ... */ }
}

// 다른 클래스에 남은 메소드를 이동시킴으로써 책임을 분산시키세요
// ...
```

**[⬆ 맨 위로 이동](#목차)**

### 높은 응집도와 낮은 결합도

응집도는 클래스 멤버가 서로에게 연관되어 있는 정도를 정의합니다. 이상적으로, 클래스 안의 모든 필드는 각 메소드에 의해 사용되어야 합니다.
그 때 우리는 클래스가 *최대한으로 응집되어있다*라고 말합니다. 이것은 항상 가능하지도 않고 권장하지 않습니다. 그러나 응집도를 높이는 것을 선호해야 합니다.

결합도는 두 클래스가 얼마나 서로에게 관련되어있거나 종속적인 정도를 뜻합니다. 하나의 클래스의 변경이 다른 클래스에게 영향을 주지 않는다면 그 클래스들의 결합도는 낮다고 말합니다.

좋은 소프트웨어 설계는 **높은 응집도**와 **낮은 결합도**를 가집니다.

**Bad:**

```ts
class UserManager {
  // Bad: 각 private 변수는 메소드의 하나 혹은 또 다른 그룹에 의해 사용됩니다.
  // 클래스가 단일 책임 이상의 책임을 가지고 있다는 명백한 증거입니다.
  // 사용자의 트랜잭션을 얻기 위해 서비스를 생성하기만 하면 되는 경우,
  // 여전히 `emailSender` 인스턴스를 전달해야 합니다.
  constructor(
    private readonly db: Database,
    private readonly emailSender: EmailSender) {
  }

  async getUser(id: number): Promise<User> {
    return await db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await db.transactions.find({ userId });
  }

  async sendGreeting(): Promise<void> {
    await emailSender.send('Welcome!');
  }

  async sendNotification(text: string): Promise<void> {
    await emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**Good:**

```ts
class UserService {
  constructor(private readonly db: Database) {
  }

  async getUser(id: number): Promise<User> {
    return await this.db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await this.db.transactions.find({ userId });
  }
}

class UserNotifier {
  constructor(private readonly emailSender: EmailSender) {
  }

  async sendGreeting(): Promise<void> {
    await this.emailSender.send('Welcome!');
  }

  async sendNotification(text: string): Promise<void> {
    await this.emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**[⬆ 맨 위로 이동](#목차)**

### 상속(inheritance)보다 조합(composition)을 사용하세요

Gang of Four의 [디자인 패턴](https://en.wikipedia.org/wiki/Design_Patterns)에 나와있듯이 할 수 있는 대로 *상속보다 조합을 사용해야 합니다.* 상속과 조합을 사용해야 하는 좋은 이유들은 각각 많습니다. 이 교훈에서 중요한 점은 당신의 마음이 본능적으로 상속을 추구한다면, 조합이 당신의 문제를 더 좋게 해결할 수 있을지 고민해보라는 것입니다. 어떤 경우에는 더 좋을 수 있습니다.  

당신은 "언제 상속을 사용해야 할까요?" 라고 의문점을 가질 것입니다. 그것은 당면한 문제에 달려 있습니다. 조합보다 상속이 더 좋은 경우가 아래에 있습니다:

1. "has-a" 관계가 아닌 "is-a" 관계일 때 (사람->동물 vs 사용자->사용자 정보)

2. 기반이 되는 클래스로부터 코드를 재사용할 수 있을 때 (사람은 모든 동물처럼 움직일 수 있습니다.)

3. 기반이 되는 클래스를 변경하여 파생된 클래스를 전체적으로 변경하려는 경우 (모든 동물은 움직일 때 칼로리가 소비됩니다.)

**Bad:**

```ts
class Employee {
  constructor(
    private readonly name: string,
    private readonly email: string) {
  }

  // ...
}

// `Employee`가 세금 데이터를 가지기 때문에 나쁜 에입니다. `EmployeeTaxData`는 `Employee`의 타입이 아닙니다.
class EmployeeTaxData extends Employee {
  constructor(
    name: string,
    email: string,
    private readonly ssn: string,
    private readonly salary: number) {
    super(name, email);
  }

  // ...
}
```

**Good:**

```ts
class Employee {
  private taxData: EmployeeTaxData;

  constructor(
    private readonly name: string,
    private readonly email: string) {
  }

  setTaxData(ssn: string, salary: number): Employee {
    this.taxData = new EmployeeTaxData(ssn, salary);
    return this;
  }

  // ...
}

class EmployeeTaxData {
  constructor(
    public readonly ssn: string,
    public readonly salary: number) {
  }

  // ...
}
```

**[⬆ 맨 위로 이동](#목차)**

### 메소드 체이닝을 사용하세요

이 패턴은 매우 유용하고 많은 라이브러리에서 공통적으로 사용하고 있습니다. 이것은 당신의 코드를 표현력이 있게 해주고 덜 장황하게 해줍니다. 이러한 이유로 메소드 체이닝을 사용해서 당신의 코드가 얼마나 명료해지는지 살펴보시길 바랍니다.

**Bad:**

```ts
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): void {
    this.collection = collection;
  }

  page(number: number, itemsPerPage: number = 100): void {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
  }

  orderBy(...fields: string[]): void {
    this.orderByFields = fields;
  }

  build(): Query {
    // ...
  }
}

// ...

const queryBuilder = new QueryBuilder();
queryBuilder.from('users');
queryBuilder.page(1, 100);
queryBuilder.orderBy('firstName', 'lastName');

const query = queryBuilder.build();
```

**Good:**

```ts
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): this {
    this.collection = collection;
    return this;
  }

  page(number: number, itemsPerPage: number = 100): this {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
    return this;
  }

  orderBy(...fields: string[]): this {
    this.orderByFields = fields;
    return this;
  }

  build(): Query {
    // ...
  }
}

// ...

const query = new QueryBuilder()
  .from('users')
  .page(1, 100)
  .orderBy('firstName', 'lastName')
  .build();
```

**[⬆ 맨 위로 이동](#목차)**

## SOLID

### 단일 책임 원칙 (SRP)

클린 코드에서 말하듯이, "클래스를 변경할 때는 단 한 가지 이유만 존재해야 합니다". 여행갈 때 가방 하나에 많은 것을 챙기는 것과 같이, 클래스를 많은 기능으로 꽉 채우고 싶은 유혹이 있습니다. 이러한 문제는 당신의 클래스가 개념적으로 응집력이 있지 않으며 변경될 많은 이유가 존재한다는 것을 말합니다. 클래스를 변경하는 많은 시간을 줄이는 것은 중요합니다. 왜냐하면 너무 많은 기능이 한 클래스에 있고 그 안에서 하나의 기능을 수정한다면, 다른 종속된 모듈에 어떻게 영향을 줄지 이해하는 것이 어렵기 때문입니다.

**Bad:**

```ts
class UserSettings {
  constructor(private readonly user: User) {
  }

  changeSettings(settings: UserSettings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Good:**

```ts
class UserAuth {
  constructor(private readonly user: User) {
  }

  verifyCredentials() {
    // ...
  }
}


class UserSettings {
  private readonly auth: UserAuth;

  constructor(private readonly user: User) {
    this.auth = new UserAuth(user);
  }

  changeSettings(settings: UserSettings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ 맨 위로 이동](#목차)**

### 개방 폐쇄 원칙 (OCP)

Bertrand Meyer가 말했듯이, "소프트웨어 엔터티(클래스, 모듈, 함수 등)는 상속에 개방되어 있습니다. 하지만 수정에는 폐쇄되어 있습니다." 이것이 무엇을 의미할까요? 이 원칙은 기본적으로 기존의 코드를 변경하지 않고 새로운 기능을 추가할 수 있도록 하는 것을 명시합니다.

**Bad:**

```ts
class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {
  }

  async fetch<T>(url: string): Promise<T> {
    if (this.adapter instanceof AjaxAdapter) {
      const response = await makeAjaxCall<T>(url);
      // response 값을 변경하고 반환합니다.
    } else if (this.adapter instanceof NodeAdapter) {
      const response = await makeHttpCall<T>(url);
      // response 값을 변경하고 반환합니다.
    }
  }
}

function makeAjaxCall<T>(url: string): Promise<T> {
  // 서버에 요청하고 프로미스를 반환합니다.
}

function makeHttpCall<T>(url: string): Promise<T> {
  // 서버에 요청하고 프로미스를 반환합니다.
}
```

**Good:**

```ts
abstract class Adapter {
  abstract async request<T>(url: string): Promise<T>;

  // 하위 클래스와 공유하는 코드 ...
}

class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T>{
    // 서버에 요청하고 프로미스를 반환합니다.
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T>{
    // 서버에 요청하고 프로미스를 반환합니다.
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {
  }

  async fetch<T>(url: string): Promise<T> {
    const response = await this.adapter.request<T>(url);
    // response 값을 변경하고 반환합니다.
  }
}
```

**[⬆ 맨 위로 이동](#목차)**

### 리스코프 치환 원칙 (LSP)

매우 단순한 개념을 뜻하는 어려워보이는 용어입니다. "만약 S가 T의 하위 타입이라면, T 타입의 객체는 S 타입의 객체로 대체될 수 있습니다. (예: S 타입 객체는 T 타입 객체로 치환될 수도 있습니다.) 이는 프로그램이 갖추어야할 속성(정확성, 수행되는 작업 등)을 변경하지 않아도 대체될 수 있습니다." 더욱 어려워보이는 정의입니다.

이를 위한 최고의 설명은 다음과 같습니다. 만약 부모 클래스와 자식 클래스가 있다면, 부모 클래스와 자식 클래스는 잘못된 결과 없이 서로 교환하여 사용될 수 있습니다. 여전히 혼란스러울 수 있습니다. 고전적인 정사각형-직사각형 예제를 살펴보세요. 수학적으로, 정사각형은 직사각형입니다. 그러나 상속을 통해 "is-a" 관계로 설계한다면, 당신은 곤경에 빠질 수 있습니다.

**Bad:**

```ts
class Rectangle {
  constructor(
    protected width: number = 0,
    protected height: number = 0) {

  }

  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  setWidth(width: number): this {
    this.width = width;
    return this;
  }

  setHeight(height: number): this {
    this.height = height;
    return this;
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width: number): this {
    this.width = width;
    this.height = width;
    return this;
  }

  setHeight(height: number): this {
    this.width = height;
    this.height = height;
    return this;
  }
}

function renderLargeRectangles(rectangles: Rectangle[]) {
  rectangles.forEach((rectangle) => {
    const area = rectangle
      .setWidth(4)
      .setHeight(5)
      .getArea(); // BAD: `Square` 클래스에서는 25를 반환합니다. 20이 반환되어야 합니다.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Good:**

```ts
abstract class Shape {
  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  abstract getArea(): number;
}

class Rectangle extends Shape {
  constructor(
    private readonly width = 0,
    private readonly height = 0) {
    super();
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(private readonly length: number) {
    super();
  }

  getArea(): number {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes: Shape[]) {
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ 맨 위로 이동](#목차)**

### 인터페이스 분리 원칙 (ISP)

인터페이스 분리 원칙은 "클라이언트는 사용하지 않는 인터페이스에 의존하지 않는다" 라는 것입니다. 이 원칙은 단일 책임 원칙과 많은 관련이 있습니다.
이 말의 뜻은 클라이언트가 노출된 메소드를 사용하는 대신에 전체 파이를 얻지 않는 방식으로 추상화를 설계해야 한다는 것입니다. 그것은 또한 클라이언트에게 클라이언트가 실제로 필요하지 않은 메소드의 구현을 강요하는 것도 포함합니다.

**Bad:**

```ts
interface SmartPrinter {
  print();
  fax();
  scan();
}

class AllInOnePrinter implements SmartPrinter {
  print() {
    // ...
  }  
  
  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements SmartPrinter {
  print() {
    // ...
  }  
  
  fax() {
    throw new Error('Fax not supported.');
  }

  scan() {
    throw new Error('Scan not supported.');
  }
}
```

**Good:**

```ts
interface Printer {
  print();
}

interface Fax {
  fax();
}

interface Scanner {
  scan();
}

class AllInOnePrinter implements Printer, Fax, Scanner {
  print() {
    // ...
  }  
  
  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements Printer {
  print() {
    // ...
  }
}
```

**[⬆ 맨 위로 이동](#목차)**

### 의존성 역전 원칙 (DIP)

이 원칙은 두 가지 필수적인 사항을 명시합니다:

1. 상위 레벨의 모듈은 하위 레벨의 모듈에 의존하지 않아야 합니다. 두 모듈은 모두 추상화에 의존해야합니다.

2. 추상화는 세부사항에 의존하지 않아야 합니다. 세부사항은 추상화에 의존해야 합니다.

처음에 바로 이해하기는 어려울 수 있습니다. Angular를 사용해봤다면, 의존성 주입(DI)의 형태 안에서 이 원칙의 구현을 확인해봤을 것입니다. 동일한 개념은 아니지만, DIP는 상위 레벨의 모듈이 하위 레벨의 모듈의 세부사항에 접근하고 설정하지 못하도록 지킵니다. DI를 통해서도 마찬가지로 성취할 수 있습니다. 이것의 큰 장점은 모듈 사이의 결합도를 줄일 수 있다는 것입니다. 결합도는 코드를 리팩토링하기 어렵게 하기 때문에 매우 나쁜 개발 패턴입니다.

DIP는 주로 IoC 컨테이너를 사용함으로써 달성됩니다. 타입스크립트를 위한 강력한 IoC 컨테이너의 예제는 [InversifyJs](https://www.npmjs.com/package/inversify)입니다.

**Bad:**

```ts
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
}

class XmlFormatter {
  parse<T>(content: string): T {
    // XML 문자열을 T 객체로 변환
  }
}

class ReportReader {

  // BAD: 특정 요청의 구현에 의존하는 것을 만들었습니다.
  // `parse` 메소드에 의존하는 `ReportReader`를 만들어야 합니다.
  private readonly formatter = new XmlFormatter();

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader();
await report = await reader.read('report.xml');
```

**Good:**

```ts
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
}

interface Formatter {
  parse<T>(content: string): T;
}

class XmlFormatter implements Formatter {
  parse<T>(content: string): T {
    // XML 문자열을 T 객체로 변환
  }
}


class JsonFormatter implements Formatter {
  parse<T>(content: string): T {
    // JSON 문자열을 T 객체로 변환
  }
}

class ReportReader {
  constructor(private readonly formatter: Formatter) {
  }

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader(new XmlFormatter());
await report = await reader.read('report.xml');

// 또는 json 보고서가 필요한 경우
const reader = new ReportReader(new JsonFormatter());
await report = await reader.read('report.json');
```

**[⬆ 맨 위로 이동](#목차)**

## 테스트

테스트는 배포보다 중요합니다. 테스트가 없거나 부족한 경우, 코드를 배포할 때마다 당신은 어떤 것이 작동하지 않을지 확실하지 않을 것입니다.
적절한 양의 테스트를 구성하는 것은 당신의 팀에게 달려있지만, (모든 문장과 브랜치에서) 100%의 커버리지를 가진다면 매우 높은 자신감과 마음의 평화를 얻을 것입니다. 이는 훌륭한 테스트 프레임워크뿐만 아니라, 좋은 [커버리지 도구](https://github.com/gotwarlost/istanbul)를 사용해야 한다는 것을 의미합니다.

테스트를 작성하지 않을 이유는 없습니다. 타입스크립트의 타입을 지원하는 [많은 양의 좋은 자바스크립트 테스트 프레임워크](http://jstherightway.org/#testing-tools)가 있으므로 당신의 팀이 선호하는 것을 찾아 사용하세요. 당신의 팀에 적합한 테스트 프레임워크를 찾았다면, 당신이 만드는 모든 새로운 기능/모듈을 위한 테스트를 항상 작성하는 것을 목표로 하세요. 테스트 기반 개발(TDD)이 당신이 선호하는 방법이라면, 매우 좋습니다. 하지만 중요한 건 어떤 기능을 만들거나 기존의 것을 리팩토링하기 전에 목표하는 커버리지를 달성하는 것입니다.

### TDD의 세 가지 법칙

1. 실패하는 단위 테스트를 작성하기 전에는 실제 코드를 작성하지 마세요.

2. 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성하세요.

3. 실패하는 단위 테스트를 통과할 정도로만 실제 코드를 작성하세요.

**[⬆ 맨 위로 이동](#목차)**

### F.I.R.S.T 규칙

명료한 테스트는 다음 규칙을 따라야 합니다:

- **Fast** 테스트는 빈번하게 실행되므로 빨라야 합니다.

- **Independent** 테스트는 서로 종속적이지 않습니다. 독립적으로 실행하든지 순서 상관없이 모두 실행하든지 동일한 결과가 나와야 합니다.

- **Repeatable** 테스트는 어떤 환경에서든 반복될 수 있습니다. 테스트가 실패하는데에 이유가 없어야 합니다.

- **Self-Validating** 테스트는 *통과* 혹은 *실패*로 답해야 합니다. 테스트가 통과되었다면 로그 파일을 보며 비교할 필요는 없습니다.

- **Timely** 단위 테스트는 실제 코드를 작성하기 전에 작성해야 합니다. 실제 코드를 작성한 후에 테스트를 작성한다면, 테스트를 작성하는 것이 너무 고단하게 느껴질 것입니다.

**[⬆ 맨 위로 이동](#목차)**

### 테스트 하나에 하나의 개념을 작성하세요

또한, 테스트는 *단일 책임 원칙*을 따라야 합니다. 단위 테스트 하나당 하나의 assert 구문을 작성하세요.

**Bad:**

```ts
import { assert } from 'chai';

describe('AwesomeDate', () => {
  it('handles date boundaries', () => {
    let date: AwesomeDate;

    date = new AwesomeDate('1/1/2015');
    assert.equal('1/31/2015', date.addDays(30));

    date = new AwesomeDate('2/1/2016');
    assert.equal('2/29/2016', date.addDays(28));

    date = new AwesomeDate('2/1/2015');
    assert.equal('3/1/2015', date.addDays(28));
  });
});
```

**Good:**

```ts
import { assert } from 'chai';

describe('AwesomeDate', () => {
  it('handles 30-day months', () => {
    const date = new AwesomeDate('1/1/2015');
    assert.equal('1/31/2015', date.addDays(30));
  });

  it('handles leap year', () => {
    const date = new AwesomeDate('2/1/2016');
    assert.equal('2/29/2016', date.addDays(28));
  });

  it('handles non-leap year', () => {
    const date = new AwesomeDate('2/1/2015');
    assert.equal('3/1/2015', date.addDays(28));
  });
});
```

**[⬆ 맨 위로 이동](#목차)**

### 테스트의 이름은 테스트의 의도가 드러나야 합니다

테스트가 실패할 때, 테스트의 이름은 어떤 것이 잘못되었는지 볼 수 있는 첫 번째 표시입니다.

**Bad:**

```ts
describe('Calendar', () => {
  it('2/29/2020', () => {
    // ...
  });

  it('throws', () => {
    // ...
  });
});
```

**Good:**

```ts
describe('Calendar', () => {
  it('should handle leap year', () => {
    // ...
  });

  it('should throw when format is invalid', () => {
    // ...
  });
});
```

**[⬆ 맨 위로 이동](#목차)**

## 동시성

### 프로미스 vs 콜백

콜백은 명료하지 않고, 지나친 양의 중첩된 *콜백 지옥*을 유발할 수 있습니다.  
콜백 방식을 사용하고 있는 기존의 함수를 프로미스를 반환하는 함수로 변환시켜주는 유틸리티 라이브러리가 있습니다.
(Node.js를 사용한다면 [`util.promisify`](https://nodejs.org/dist/latest-v8.x/docs/api/util.html#util_util_promisify_original)를 확인해주세요. 일반적인 목적이라면 [pify](https://www.npmjs.com/package/pify), [es6-promisify](https://www.npmjs.com/package/es6-promisify)를 확인해주세요.)

**Bad:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';

function downloadPage(url: string, saveTo: string, callback: (error: Error, content?: string) => void) {
  get(url, (error, response) => {
    if (error) {
      callback(error);
    } else {
      writeFile(saveTo, response.body, (error) => {
        if (error) {
          callback(error);
        } else {
          callback(null, response.body);
        }
      });
    }
  });
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html', (error, content) => {
  if (error) {
    console.error(error);
  } else {
    console.log(content);
  }
});
```

**Good:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url)
    .then(response => write(saveTo, response));
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html')
  .then(content => console.log(content))
  .catch(error => console.error(error));  
```

프로미스는 코드를 더욱 간결하게 해주는 몇몇의 헬퍼 메소드를 지원합니다:  

| 패턴                      | 설명                                        |  
| ------------------------ | -----------------------------------------  |  
| `Promise.resolve(value)` | 해결(resolve)된 프로미스로 값을 변환함.            |  
| `Promise.reject(error)`  | 거부(reject)된 프로미스로 에러를 변환함.            |  
| `Promise.all(promises)`  | 전달된 모든 프로미스가 이행한 값의 배열을 이행하는 새 프로미스 객체를 반환하거나 거부된 첫 번째 프로미스의 이유로 거부함. |
| `Promise.race(promises)`| 전달된 프로미스의 배열에서 가장 먼저 완료된 결과/에러로 이행/거부된 새 프로미스 객체를 반환함. |

`Promise.all`는 병렬적으로 작업을 수행할 필요가 있을 때 유용합니다. `Promise.race`는 프로미스를 위한 타임아웃과 같은 것을 구현하는 것을 쉽게 할 수 있도록 도와줍니다.

**[⬆ 맨 위로 이동](#목차)**

### 프로미스보다 `async`/`await`가 더 명료합니다

`async`/`await` 구문을 사용하면 연결된 프로미스 구문보다 훨씬 더 명료하고 이해하기 쉬운 코드를 작성할 수 있습니다. `async` 키워드가 앞에 붙여진 함수는  `await` 키워드에서 코드의 실행을 멈춘다는 것을 자바스크립트 런타임에게 알려줍니다.

**Bad:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = util.promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url).then(response => write(saveTo, response));
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html')
  .then(content => console.log(content))
  .catch(error => console.error(error));  
```

**Good:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

async function downloadPage(url: string, saveTo: string): Promise<string> {
  const response = await get(url);
  await write(saveTo, response);
  return response;
}

// somewhere in an async function
try {
  const content = await downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html');
  console.log(content);
} catch (error) {
  console.error(error);
}
```

**[⬆ 맨 위로 이동](#목차)**

## 에러 처리

에러를 던지는 것은 좋은 것입니다! 에러를 던진다는 것은 런타임이 당신의 프로그램에서 뭔가 잘못되었을 때 식별하고 현재 스택에서 함수 실행을 멈추고, (노드에서) 프로세스를 종료하며, 스택 트레이스를 콘솔에 보여줌으로써 당신에게 해당 에러를 알려주는 것을 의미합니다.

### `throw` 또는 `reject` 구문에서 항상 `Error` 타입을 사용하세요

타입스크립트뿐만 아니라 자바스크립트는 어떤 객체든지 에러를 `throw` 하는 것을 허용합니다. 또한, 프로미스는 어떤 객체라도 거부될 수 있습니다.  
`Error` 타입에는 `throw` 구문을 사용하는 것이 바람직합니다. 당신의 에러가 상위 코드의 `catch` 구문에서 잡힐 수 있기 때문입니다.
문자열 메시지가 잡히는 것은 매우 혼란스러우며 이는 [디버깅을 더 고통스럽게](https://basarat.gitbook.io/typescript/type-system/exceptions#always-use-error) 만듭니다.  
이와 같은 이유로 당신은 `Error` 타입으로 프로미스를 거부해야합니다.

**Bad:**

```ts
function calculateTotal(items: Item[]): number {
  throw 'Not implemented.';
}

function get(): Promise<Item[]> {
  return Promise.reject('Not implemented.');
}
```

**Good:**

```ts
function calculateTotal(items: Item[]): number {
  throw new Error('Not implemented.');
}

function get(): Promise<Item[]> {
  return Promise.reject(new Error('Not implemented.'));
}

// 또는 아래와 동일합니다:

async function get(): Promise<Item[]> {
  throw new Error('Not implemented.');
}
```

`Error` 타입을 사용하는 장점은 `try/catch/finally` 구문에 의해 지원되고 암시적으로 모든 에러가 디버깅에 매우 강력한 `stack` 속성을 가지고 있기 때문입니다.  
또 하나의 대안은 있습니다. `throw` 구문을 사용하지 않는 대신, 항상 사용자 정의 객체를 반환하는 것입니다.  
타입스크립트는 이것을 훨씬 더 쉽게 만듭니다.
아래의 예제를 확인하세요:

```ts
type Result<R> = { isError: false, value: R };
type Failure<E> = { isError: true, error: E };
type Failable<R, E> = Result<R> | Failure<E>;

function calculateTotal(items: Item[]): Failable<number, 'empty'> {
  if (items.length === 0) {
    return { isError: true, error: 'empty' };
  }

  // ...
  return { isError: false, value: 42 };
}
```

이 아이디어의 자세한 설명은 [원문](https://medium.com/@dhruvrajvanshi/making-exceptions-type-safe-in-typescript-c4d200ee78e9)을 참고하세요.

**[⬆ 맨 위로 이동](#목차)**

### `catch` 절에서 에러 처리 부분을 비워두지 마세요

`catch` 절에서 단지 에러를 받는 것만으로는 해당 에러에 대응할 수 없습니다. 또한, 콘솔에 에러를 기록하는 것(`console.log`)은 콘솔에 출력된 많은 것들 사이에서 발견되지 못할 수 있기 때문에 그다지 좋은 선택은 아닙니다. 당신이 어떤 코드를 `try/catch`로 감쌌다면, 그 코드에서 에러가 일어날 수 있으며, 즉 에러가 발생했을 때에 대한 계획이나 장치가 있어야 한다는 것을 의미합니다.

**Bad:**

```ts
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}

// 아래 예제는 훨씬 나쁩니다.

try {
  functionThatMightThrow();
} catch (error) {
  // 에러를 무시
}
```

**Good:**

```ts
import { logger } from './logging'

try {
  functionThatMightThrow();
} catch (error) {
  logger.log(error);
}
```

**[⬆ 맨 위로 이동](#목차)**

### 요청이 거부된 프로미스 객체를 무시하지 마세요

위와 같이 `try/catch` 절에서 받은 에러 처리 부분을 비워두면 안됩니다.

**Bad:**

```ts
getUser()
  .then((user: User) => {
    return sendEmail(user.email, 'Welcome!');
  })
  .catch((error) => {
    console.log(error);
  });
```

**Good:**

```ts
import { logger } from './logging'

getUser()
  .then((user: User) => {
    return sendEmail(user.email, 'Welcome!');
  })
  .catch((error) => {
    logger.log(error);
  });

// 또는 async/await 구문을 사용할 수 있습니다:

try {
  const user = await getUser();
  await sendEmail(user.email, 'Welcome!');
} catch (error) {
  logger.log(error);
}
```

**[⬆ 맨 위로 이동](#목차)**

## 서식

서식은 주관적입니다. 여기에 있는 많은 규칙들과 같이 당신이 따르기 어려운 규칙은 없습니다. 중요한 점은 서식에 대해서 *논쟁하지 않는 것*입니다. 서식을 자동화하기 위한 도구들이 매우 많습니다. 그 중 하나를 사용하세요! 서식에 대해 논쟁하는 것은 엔지니어에게 시간과 돈 낭비일 뿐입니다. 따라야하는 일반적인 규칙은 *일관적인 서식 규칙을 지켜야하는 것입니다*.

[TSLint](https://palantir.github.io/tslint/)라고 불리는 타입스크립트를 위한 강력한 도구가 있습니다. 이것은 코드의 가독성과 유지보수성을 극적으로 개선시키도록 도와주는 정적 분석 도구입니다.
프로젝트에 참고할 수 있는 TSLint 설정을 사용할 준비가 되었습니다:

- [TSLint Config Standard](https://www.npmjs.com/package/tslint-config-standard) - 표준 스타일 규칙

- [TSLint Config Airbnb](https://www.npmjs.com/package/tslint-config-airbnb) - 에어비엔비 스타일 가이드

- [TSLint Clean Code](https://www.npmjs.com/package/tslint-clean-code) - [Clean Code: A Handbook of Agile Software Craftsmanship](https://www.amazon.ca/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)에 영감 받은 TSLint 규칙

- [TSLint react](https://www.npmjs.com/package/tslint-react) - React & JSX와 관련된 lint 규칙

- [TSLint + Prettier](https://www.npmjs.com/package/tslint-config-prettier) - [Prettier](https://github.com/prettier/prettier) 코드 포맷터를 위한 lint 규칙

- [ESLint rules for TSLint](https://www.npmjs.com/package/tslint-eslint-rules) - 타입스크립트를 위한 ESLint 규칙

- [Immutable](https://www.npmjs.com/package/tslint-immutable) - 타입스크립트에서 변경을 허락하지 않는 규칙

또한, 훌륭한 자료인 [타입스크립트 스타일 가이드와 코딩 컨벤션](https://basarat.gitbook.io/typescript/styleguide)을 참고해주세요.

> 역자주: TSLint는 deprecated되었습니다. [Roadmap: TSLint -> ESLint](https://github.com/palantir/tslint/issues/4534) 이슈를 확인해주세요.

### 일관적으로 대소문자를 사용하세요

대소문자를 구분하여 작성하는 것은 당신에게 변수, 함수 등에 대해서 많은 것을 알려줍니다. 이 규칙은 주관적이어서, 당신의 팀이 원하는 것을 선택해야 합니다. 중요한 점은 어떤 걸 선택하였든지 간에 *일관적*이어야 한다는 것입니다.

**Bad:**

```ts
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

type animal = { /* ... */ }
type Container = { /* ... */ }
```

**Good:**

```ts
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const ARTISTS = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restoreDatabase() {}

type Animal = { /* ... */ }
type Container = { /* ... */ }
```

클래스, 인터페이스, 타입 그리고 네임스페이스 이름에는 `PascalCase`를 사용하세요.  
변수, 함수 그리고 클래스 멤버 이름에는 `camelCase`를 사용하세요.

**[⬆ 맨 위로 이동](#목차)**

### 함수 호출자와 피호출자를 가깝게 위치시키세요

함수가 다른 함수를 호출할 때, 코드에서 이 함수들을 수직적으로 가깝게 유지하도록 하세요. 이상적으로는, 호출하는 함수를 호출을 당하는 함수 바로 위에 위치시키는게 좋습니다.
우리는 신문처럼 코드를 위에서 아래로 읽는 경향이 있기 때문에, 코드를 작성할 때에도 이런 방식으로 읽는 것을 고려해야 합니다.

**Bad:**

```ts
class PerformanceReview {
  constructor(private readonly employee: Employee) {
  }

  private lookupPeers() {
    return db.lookup(this.employee.id, 'peers');
  }

  private lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**Good:**

```ts
class PerformanceReview {
  constructor(private readonly employee: Employee) {
  }

  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  private lookupPeers() {
    return db.lookup(this.employee.id, 'peers');
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**[⬆ 맨 위로 이동](#목차)**

### import 구문을 특정 순서대로 정리하세요

`import` 구문을 읽기 쉽고 명료하게 하면 당신은 현재 코드의 의존성을 빠르게 확인할 수 있습니다. 다음과 같은 `import` 구문 정리를 위한 좋은 방법들을 적용해보세요:

- import 구문은 알파벳 순서대로 배열하고 그룹화해야 합니다.
- 사용하지 않은 import 구문은 제거되어야 합니다.
- 이름이 있는 import 구문은 알파벳 순서대로 배열해야 합니다. (예: `import {A, B, C} from 'foo';`)
- import 하는 소스코드는 그룹 내에서 알파벳 순서대로 배열해야 합니다. (예: `import * as foo from 'a'; import * as bar from 'b';`)
- import 구문의 그룹은 빈 줄로 구분되어야 합니다.
- 그룹은 다음 순서를 준수해야 합니다:
  - 폴리필 (예: `import 'reflect-metadata';`)
  - Node 내장 모듈 (예: `import fs from 'fs';`)
  - 외부 모듈 (예: `import { query } from 'itiriri';`)
  - 내부 모듈 (예: `import { UserService } from 'src/services/userService';`)
  - 상위 디렉토리에서 불러오는 모듈 (예: `import foo from '../foo'; import qux from '../../foo/qux';`)
  - 동일한 계층의 디렉토리에서 불러오는 모듈 (예: `import bar from './bar'; import baz from './bar/baz';`)

**Bad:**

```ts
import { TypeDefinition } from '../types/typeDefinition';
import { AttributeTypes } from '../model/attribute';
import { ApiCredentials, Adapters } from './common/api/authorization';
import fs from 'fs';
import { ConfigPlugin } from './plugins/config/configPlugin';
import { BindingScopeEnum, Container } from 'inversify';
import 'reflect-metadata';
```

**Good:**

```ts
import 'reflect-metadata';

import fs from 'fs';
import { BindingScopeEnum, Container } from 'inversify';

import { AttributeTypes } from '../model/attribute';
import { TypeDefinition } from '../types/typeDefinition';

import { ApiCredentials, Adapters } from './common/api/authorization';
import { ConfigPlugin } from './plugins/config/configPlugin';
```

**[⬆ 맨 위로 이동](#목차)**

### 타입스크립트 앨리어스를 사용하세요

`tsconfig.json`의 `compilerOptions` 섹션 안에서 `paths`와 `baseUrl` 속성을 정의해 더 보기 좋은 import 구문을 작성해주세요.

이 방법은 import 구문을 사용할 때 긴 상대경로를 작성하는 것을 피하게 도와줄 것입니다.

**Bad:**

```ts
import { UserService } from '../../../services/UserService';
```

**Good:**

```ts
import { UserService } from '@services/UserService';
```

```js
// tsconfig.json
...
  "compilerOptions": {
    ...
    "baseUrl": "src",
    "paths": {
      "@services": ["services/*"]
    }
    ...
  }
...
```

**[⬆ 맨 위로 이동](#목차)**

## 주석

주석을 사용하는 것은 주석 없이 코드를 작성하는 것이 실패했다는 표시입니다. 코드는 단일 진실 공급원(Single source of truth)이어야 합니다.

> 나쁜 코드에 주석들 달지 마라. 새로 짜라.  
> — *Brian W. Kernighan and P. J. Plaugher*

### 주석 대신에 자체적으로 설명 가능한 코드를 작성하세요

주석은 변명일 뿐, 필요하지 않습니다. 좋은 코드는 대부분 그 존재 자체로 문서화가 됩니다.

**Bad:**

```ts
// subscription이 활성화 상태인지 체크합니다.
if (subscription.endDate > Date.now) {  }
```

**Good:**

```ts
const isSubscriptionActive = subscription.endDate > Date.now;
if (isSubscriptionActive) { /* ... */ }
```

**[⬆ 맨 위로 이동](#목차)**

### 당신의 코드를 주석 처리하지 마세요

버전 관리 시스템이 존재하는 이유입니다. 사용하지 않는 코드는 기록에 남기세요.

**Bad:**

```ts
type User = {
  name: string;
  email: string;
  // age: number;
  // jobPosition: string;
}
```

**Good:**

```ts
type User = {
  name: string;
  email: string;
}
```

**[⬆ 맨 위로 이동](#목차)**

### 일기 같은 주석을 달지 마세요

버전 관리 시스템을 사용하세요! 죽은 코드, 주석 처리된 코드, 특히 일기 같은 주석은 필요 없습니다. 대신에 기록을 보기 위해 `git log` 명령어를 사용하세요!

**Bad:**

```ts
/**
 * 2016-12-20: 이해하지 못해서 모나드를 제거함 (RM)
 * 2016-10-01: 특별한 모나드를 사용해 개선함 (JP)
 * 2016-02-03: 타입 체킹 추가함 (LI)
 * 2015-03-14: combine 함수를 구현함 (JR)
 */
function combine(a: number, b: number): number {
  return a + b;
}
```

**Good:**

```ts
function combine(a: number, b: number): number {
  return a + b;
}
```

**[⬆ 맨 위로 이동](#목차)**

### 코드의 위치를 설명하는 주석을 사용하지 마세요

이건 보통 코드를 어지럽히기만 합니다. 함수와 변수 이름을 적절한 들여쓰기와 서식으로 당신의 코드에 시각적인 구조가 보이도록 하세요.  
대부분의 IDE(통합 개발 환경)에서는 코드 블록을 `접기/펼치기` 할 수 있는 기능을 지원합니다. (Visual Studio Code의 [folding regions](https://code.visualstudio.com/updates/v1_17#_folding-regions)를 확인해보세요).

**Bad:**

```ts
////////////////////////////////////////////////////////////////////////////////
// Client 클래스
////////////////////////////////////////////////////////////////////////////////
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  ////////////////////////////////////////////////////////////////////////////////
  // public 메소드
  ////////////////////////////////////////////////////////////////////////////////
  public describe(): string {
    // ...
  }

  ////////////////////////////////////////////////////////////////////////////////
  // private 메소드
  ////////////////////////////////////////////////////////////////////////////////
  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
};
```

**Good:**

```ts
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  public describe(): string {
    // ...
  }

  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
};
```

**[⬆ 맨 위로 이동](#목차)**

### TODO 주석

추후에 개선을 위해 코드에 메모를 남겨야할 때, `// TODO` 주석을 사용하세요. 대부분의 IDE는 이런 종류의 주석을 특별하게 지원하기 때문에 해야할 일 목록을 빠르게 검토할 수 있습니다.

하지만 *TODO* 주석이 나쁜 코드를 작성할 이유는 아니라는 것을 명심하세요.

**Bad:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**Good:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // TODO: ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**[⬆ 맨 위로 이동](#목차)**

## 번역

이 글을 다른 언어로도 읽을 수 있습니다:  
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [vitorfreitas/clean-code-typescript](https://github.com/vitorfreitas/clean-code-typescript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese**: 
  - [beginor/clean-code-typescript](https://github.com/beginor/clean-code-typescript)
  - [pipiliang/clean-code-typescript](https://github.com/pipiliang/clean-code-typescript)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [MSakamaki/clean-code-typescript](https://github.com/MSakamaki/clean-code-typescript)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [Real001/clean-code-typescript](https://github.com/Real001/clean-code-typescript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [ozanhonamlioglu/clean-code-typescript](https://github.com/ozanhonamlioglu/clean-code-typescript)
- ![ko](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [738/clean-code-typescript](https://github.com/738/clean-code-typescript)

번역이 완료되면 참고문헌에 추가됩니다.
자세한 내용과 진행상황을 보고싶다면 이 [논의](https://github.com/labs42io/clean-code-typescript/issues/15)를 확인하세요.
당신은 당신의 언어에 이 글을 번역함으로써 *클린 코드* 커뮤니티에 중요한 기여를 할 수 있습니다.

**[⬆ 맨 위로 이동](#목차)**

## 번역에 도움을 주신 분들

### 리뷰

- [@samslow](https://github.com/samslow)
- [@Space0726](https://github.com/Space0726)
- [@zwonlala](https://github.com/zwonlala)
- [@joonghyeob-shin](https://github.com/joonghyeob-shin)

### 참고자료

- [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
