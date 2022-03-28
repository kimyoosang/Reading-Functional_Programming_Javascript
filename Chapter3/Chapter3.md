# Chapter3. 자료구조는 적게, 일은 더 많이

## **3.1 애플리케이션의 제어 흐름**

- 프로그램이 정답에 이르기까지 거치는 경로를 **제어 흐름**이라고 한다
- **명령형 프로그램**은 작업 수행에 필요한 전 단계를 노출하여 흐름이나 경로를 아주 자세히 서술한다. 보통 작업을 수행하는 단계는 루프와 분기문, 구문마다 값이 바뀌는 변수들로 빼곡히 들어찬다
- **선언적 프로그램, 특히 함수형 프로그램**은 독립적인 블랙박스 연산들이 단순하게, 즉 최소한의 제어 구조를 통해 연결되어 추상화 수준이 높다. 이렇게 연결한 연산들은 각자 다음 연산으로 상태를 이동시키는 고계함수에 불과하다. 실제로 함수형 프로그램은 데이터와 제어 흐름 자체를 고수준 컴포넌트 사이의 단순한 연결로 취급한다
- 연산을 체이닝하면 간결하면서 물 흐르는 듯한, 표현적인 형태로 프로그램을 작성할 수 있어 제어 흐름과 계산 로직을 분리할 수 있고 코드와 데이터를 더욱 효과적으로 헤아릴 수 있다

## **3.2 메서드 페이닝**

- **메서드 체이닝**은 여러 메서드를 단일 구분으로 호출하는 OOP패턴이다. 메서드가 모두 동일한 객체에 속해 있으면 메서드 흘리기 라고도 한다
- 함수 코드를 안쪽에서 바깥쪽으로 작성하면 메서드 체이닝 방식만큼 매끄럽지 못하다. 로직을 파악하려면 가장 안쪽에 감싼 함수부터 한 꺼풀씩 벗겨내야 하고 가독성도 현저히 떨어진다
- 변이를 일으키지 않는 한 함수형 프로그래밍에서도 단일 개게 인스턴스에 속한 메서드를 체이닝하는 건 나름대로 쓸모가 있다

## **3.3 함수 체이닝**

- 객체지향 프로그램은 주로 상속을 통해 코드를 재사용한다. 순수 객체지향 언어에서, 특히 언어 자체의 자료구조를 구현한 코드를 보면 부모의 상태 및 메서드를 모두 물려받는 패턴이 자주 보인다
- 함수형 프로그래밍은 객체지향 프로그램과 접근 방법이 다르다. 자료구조를 새로 만들어 어떤 요건을 충족시키는게 아니라, 배열 드으이 흔한 자료구조를 이용해 다수의 굵게 나뉜 고계 연산을 적용한다
- 고계 연산으로 다음과 같은 일을 한다
  1. 작업을 수행하기 위해 무슨 일을 해야 하는지 기술된 함수를 인수로 받는다
  2. 임시 변수의 값을 계속 바꾸면서 부수효과를 일으키는 기존 수동 루프를 대체한다. 그 결과 관리할 코드가 줄고 에러가 날 만한 코드 역시 줄어든다

### **3.3.1 람다 표현식**

- 함수형 프로그래밍에서 탄생한 **람다 표현식**(자바스크립트에서는 **두 줄 화살표 함수**라고도 함)은 한 줄짜리 익명 함수를 일반 함수 선언보다 단축된 구분으로 나나낸다. 람다 함수는 여러 줄로도 표기할 수 있지만, 거의 대부분 한줄로 쓴다
- 람다 함수든, 일반 함수든 코드 가독성의 차이만 있을 뿐, 실제 하는 일은 같다
- 예 1: 사람 이름을 추출하는 코드

  ```javascript
  const name = (p) => p.fullname;
  console.log(name(p1));
  ```

  - (p) => p.fullname은 매개변수를 p를 받아 p.fullname을 반환함을 의미하는 간편 구분이다

- 람다 표현식은 항상 어떤 값을 반환하게 만들어 함수 정의부를 확실히 함수형으로 굳힌다. 한 줄자리 표현식의 반환값은 함수 본체를 실행한 결과값이다. 여기서 주목할 점은 일급 함수와 람다 표현식의 관계다
- 위 예제에서 name은 실재하는 값이 아니라, 그 값을 얻기 위한 방법을 가리킨다. 즉 name으로 데이터를 계산하는 로직이 담긴 두 줄 화살표 함수를 가리키는 것이다. 함수형 프로그램은 이렇게 함수를 마치 값처럼 쓸 수 있다
- 함수형 프로그램은 람다 표현식과 잘 어올리는 세 주요 고계함수 map, reduce, filter를 적극 사용할 것을 권장한다. 사실 함수형 자바스크립트는 대부분 다료 리스트를 처리하는 코드이다
- 로대시JS는 개발자가 함수형 프로그램을 작성하도록 유도하는 중요한 장치를 제공하고, 여러 가지 공통적인 프로그래밍 작업을 처리하는 데 유용한 도우미 함수들을 풍성하게 지원한다

### **3.3.2 \_.map: 데이터를 반환**

- 큰 데이터 컬렉션의 원소를 모두 반환해야 하는 경우 사용할 수 있는 map은 배열 각 원소에 이터레이터 함수를 적용하여 크기가 같은 새 배열을 반환한다

- 예 2-1: 데이터 컬렉션 원소 반환 (명령형)

  ```javascript
  var result = [];
  var persons = [p1, p2, p3, p4];

  for (let i = 0; i < persons.length; i++) {
    var p = persons[i];
    if (p !== null && p !== undefined) {
      result.push(p.fullname);
    }
  }
  ```

- 예 2-2: 데이터 컬렉션 원소 반환 (map사용)

  ```javascript
  _.map(persons, (s) => (s !== null && s !== undefined ? s.fullname : ""));
  ```

  - map함수는 루프를 쓰거나 스포크 문제를 신경쓸 필요 없이 컬렉션의 원소를 전푸 파싱할 경우 아주 유용하다
  - 항상 새로운 배열을 반환하므로 불변성도 간직된다
  - map은 함수 f와 n개의 원소가 담긴 컬렉션을 받아 왼쪽 -> 오른쪽 방향으로 각 원소에 f를 적용한 계산 결과를, 크기가 n인 새 배열에 담아 반환한다

- 예 3: map 구현부

  ```javascript
  function map(arr, fn) {
    const len = arr.length,
          result = new Array(len)
    for(let idx = 0l idx < len; ++idx) {
      result[idx] = fn(arr[idx], dix, arr)
    }
    return result
  }
  ```

  - map연산은 무조건 왼쪽 -> 오른쪽 방향으로 진행한다
  - 오른쪽 -> 왼쪽 방향으로 진행하려면 배열 원소를 버꾸로 뒤집어야 하는데 로대시JS는 일관성을 유지하기 위해 자바스크립트의 Array.reverse()에 해당하는 \_.reverse()메서드를 지원한다. 이 함수는 원본 배열에 변이를 일으키므로 개발자는 부수효과가 언제 일어날지 알고 있어야 한다

  ```javascript
  _(persons)
    .reverse()
    .map((p) => (p !== null && p !== undefined ? p.fullname : ""));
  ```

  - 원하는 객체를 \_(...)로 감싸면 로대시JS의 함수형 도구를 이용해 변환할 수 있다

### **3.3.3 \_.reduce: 결과를 수집**

- 데이터를 변환한 후에는 변환된 데이터로부터 의미 있는 결과를 도출해야하는데 이때 reduce함수가 유용하다
- reduce는 원소 배열을 하나의 값으로 짜내는 고계함수로, 원소마다 함수를 실행한 결과값의 누적치를 계산한다

- 예 4: reduce 구현부

  ```javascript
  function reduce(arr, fn, accumulator) {
    let idx = -1;
    len = arr.length;

    if (!accumulator && len > 0) {
      accumulator = arr[++idx];
    }

    while (++idx < len) {
      accumulator = fn(accumulator, arr[idx], idx, arr);
    }
    return accumulator;
  }
  ```

- reduce는 다음 매개변수를 받는다

  1. fn

  - 배열 각 원소마다 실행할 이터레이터 함수로, 매개변수는 누산치, 현재 값, 인덱스, 배열이다

  2. accumulator

  - 계산할 초깃값으로 넘겨받는 인수이고, 함수 호출을 거치며 매 호출 시 계산된 결과값을 저장하는 데 쓰인다

- 예 5: 국가별 인구 계산

  ```javascript
  _(persons).reduce((stat, person)=>{
    const country = person.address.country
    stat[country] = _.isUndifined(stat(country)?1:stat[country + 1]
    return stat
    )
  }, {})

  ```

- 많이 쓰이는 맵-리듀스 조합을 이용하면 작업을 더 단순화할 수 있다. 원하는 기능을 map,reduce 두 함수에 매개변수로 담아 보내고 이들을 연결해서 기능을 확장하는 거싱다

- 예 6: map과 reduce를 조합하여 통계치를 산출

  ```javascript
  const getCountry = (person) => person.address.country;

  const gatherStats = function (stat, criteria) {
    stat[criteria] = _.isUndefined(stat[criteria]) ? 1 : stat[critaria] + 1;
    return stat;
  };

  _(person).map(getCountry).reduce(getherStats, {});
  ```

  - map으로 객체 배열을 처리하여 국가 정보를 뽑아낸 다음, reduce로 최종 결과를 수집한다
  - 예 5와 결과는 같지만, 훨씬 깔끔하고 확장 가능한 모양새이다

- 속성을 직접 건드리는 대신 (람다JS로) Person객체의 address.city속성에 초점을 맞춘 렌즈를 써보자

  ```javascript
  const cityPath = ["address", "city"];
  const cityLens = R.lens(R.path(cityPath), R.assocPath(cityPath));

  //거주 도시별 인구를 산출하는 작업
  _(persons).map(R.view(cityLens)).reduce(getherStats, {});

  //_.groupBy를 쓰면 코드가 훨씬 더 간단해진다
  _.groupBy(persons, R.view(cityLens));
  ```

- map과 달리 reduce는 누산치에의존하기 때문에 결합법칙이 성립하지 않는 연산은 진행 순서(왼쪽 -> 오른쪽, 오른쪽 -> 왼쪽)에 따라 결과가 달라진다
- reduce는 일괄적용 계산이라서 배열을 순회하는 도중 그만두고 나머지 원소를 생략할 방법이 없다. 가령 어떤 입력값 리스트를 검증하는 경우, 검증 결과를 하나의 불리언 값으로 리듀스하면 입력값이 전부 올라른지 알아낼 수 있을 것이다
- 그러나 reduce는 리스트 값을 빠짐없이 방문하기 때문에 다소 비효율적이다. 잘못된 입력값이 하나라도 발견되면 나머지 값들은 더 이상 체크할 필요가 없다

### \*\*3.3.4 \_.filter: 원하지 않는 원소를 제거

- 큰 데이터 컬렉셔너을 처리할 경우, 계산하지 않을 원소는 사전에 빼는 것이 좋다
- filter(select라고도 한다)는 배열 원소를 반복하면서 술어 함수 p가 true를 반환하는 원소만 추려내고 그 결과를 새 배열에 담아 반환하는 고계함수다

- 예 7: filter 구현부

  ```javascript
  function filter(arr, predicate) {
    let idx = -1;
    len = arr.length;
    result = [];

    while (++idx < len) {
      let value = arr[idx];
      if (predicate(value, dix, this)) {
        result.push(value);
      }
    }
    return result;
  }
  ```

- filter는 대상 배열과, 원소를 결과에 포함할지 결정하는 술어 함수 두 가지를 인수로 받는다
- 술어 함수 결과가 true인 원소는 남기고 그렇지 않은 원소는 내보낸다. filter는 배열에서 오류 데이터를 제거하는 용도로 자주 쓰인다

  ```javascript
  _(persons).filter(isValid).map(fullname);
  ```

- 또한 객체 컬렉션에서 어떤 값을 추리고자 할대, 조건문 대신 \_.filter를 쓰면 코드가 훨씬 간결해진다

  ```javascript
  const bornIn1903 = (person) => persons.birthYear === 1903;

  _(person).filter(bornIn1903).map(fullname).join(" and");
  ```

## **3.4 코드 헤어라기**

- '코드를 헤어린다'는 뜻은 프로그램의 일부만 들여다봐도 무슨 일을 하는 코드인지 메털 모델을 쉽게 구축할 수 있다는 의미다
- **멘털 모델**이란 전체 변수의 상태와 함수 출력 같은 동적인 부분뿐만 아니라 설계 가독성 및 표현성 같은 정적인 측면까지 포괄하는 개념이다
- 불변성과 순수함수가 이러한 멘털 모델 구축을 더 용이하게 해준다

### **3.4.1 선언적 코드와 느긋한 함수 체인**

- FP의 선언적 모델에 따르면, 프로그램이란 개별적인 순수함수들을 평가하는 과정이라고 볼 수 있다
- 그래서 필요 시 코드의 흐름성과 표현성을 높이기 위한 추상화 수단을 지원하며, 이렇게 함으로써 개발하려는 애플리케이션의 실체를 명확하게 쵸현하는 온톨로지 또는 어휘집을 만들 수 있다
- map, reduce, filter라는 구성 요소를 바탕으로 순수함수를 쌓아가면 자연스래 한눈에 봐도 흐름이 읽히는 코드가 완성된다
- 이 정도 수준으로 추상화하면 비로소 기반 자료구조에 영향을 끼치지 않는 방향으로 연산을 바라볼 수 있다. 이론적으로 말해서 배열, 연결 리스트, 이진 트리 등 어떤 자료구조를 쓰더라도 프로그램 자체 의미가 달라져선 안된다. 그래서 함수형 프로그래밍은 자료구조보다 연산에 더 중점을 둔다
- 예 1-1: 이름 리스트를 읽고 데이터를 정제 후, 중복은 제거하고 정렬하는 코드 (명령형)

  ```javascript
  //문자열 형식이 제각각인 데이터
  var names = ["alonze church", "Haskell curry", "stepthen_kleene"];

  var result = [];
  for (let i = 0; i < names.length; i++) {
    var n = names[i];
    if (n !== undefined && n !== null) {
      var ns = n.replace(/_/, " ").split(" ");
      for (let j = 0; j < ns.length; j++) {
        var p = ns[j];
        p = p.charAt(0).toUpperCase() + p.slice(1);
        ns[j] = p;
      }
      if (result.indexOf(ns.join(" ")) < 0) {
        result.push(ns.join(" "));
      }
    }
  }
  result.sort();
  ```

- 결과는 제대로 나오지만 명령형 코드의 단점은 특정 문제의 해결만을 목표한다는 것이다
- 예 1-1 역시 함수형보다 훨씬 저수준에서 추상한 코드로서 한 가지로 용도로 고정된다. 추상화 수준이 낮을수록 코드를 재사용할 기회는 줄어들고 에러 가능성과 코드 복잡성은 증가한다
- 반면, 함수형 프로그램은 블랙박스 컴포넌트르 서로 연결만 해주고, 뒷일은 테스트까지 마친 검증된 API에게 모두 맡긴다
- 예 1-2: 이름 리스트를 읽고 데이터를 정제 후, 중복은 제거하고 정렬하는 코드 (명령형)

  ```javascript
  _.chain(namse)
    .filter(isValid)
    .map((s) => s.replace(/_/, " "))
    .uniq()
    .map(_.startCase)
    .sort()
    .value();
  ```

  - names 배열을 정확한 인덱스로 순회하는 등 버거운 일은 모두 _.filter와 _.map 함수가 대행하므로 그저 나머지 단계에 대한 프로그램 로직을 구현하면 된다
  - uniq로 중복 데이터를 집어내고 \_.startCase로 각 단어의 첫자를 대문자로 바꾼다음, 자미가에 알파벳 순으로 정렬한다

예 2: 국가별 인구를 계산하는 gatherStats 코드

```javascript

const gatherStats = function(start, country) {
  if(!isValid(stat([country])) {
    stat[country] = {'name':country, 'count':0}
  }
  stat[country].count++
  return stat
}

//Person 배열에 데이터 집어넣기
const p5 = new Person('David', 'Hilbert', '555-55-5555')
p5.address = new Address('Germany')
p5.birthYear = 1903

//인구가 가장 많은 국가를 반환하는 프로그램
_.chain(persons)
  .filter(isValid).
  map(_.property('address.country'))
  .reduce(gatherStats, {})
  .values()
  .sortBy('count')
  .reverse()
  .first()
  .value()
  .name

```

- \_.chain 함수는 주어진 입력을 원하는 출력으로 변환하는 연산들을 연결함으로써 입력 객체의 상태를 확장한다
- \_(...) 객체로 단축 표기한 구분과 달리, 이 함수는 임의의 함수를 명사적으로 체이닝 가능한 함수로 만든다. 프로그램이 조금 복잡해 보이긴 하지만, 변수를 만들거나 루프를 돌리는 일 따위는 할 필요가 없다
- \_.chain을 쓰면 복잡한 프로그램을 느긋하게 작동시키는 장점도 있다. 제일 끝에서 value()함수를 호풀하기 전에는 아무것도 실행되지 않는다
- 결괏값이 필요없는 함수는 실행을 건너뛸 수 있어서 애플리케이션 성능에 엄청난 영향을 미친다
- 부드럽게 작동하는 건 FP의 근본 원리인, 부수효과 없는 순수함수 덕분이다. 체인에 속한 각 함수는 이전 단계의 함수가 제공한 새 배열에 자신의 불변 연산을 적용한다
- 프로그램 파이프라인을 느긋하게 정의하면 가독성을 비롯해 여러모로 이롭다. 느긋한 프로그램은 평가 이전에 정의하기 때문에 자료구조를 재사용하거나 메서드를 융합하여 최적화할 수 있다

### **3.4.2 유사 SQL 데이터: 데이터로서의 함수**

- map, reduce, groupBy, sortBy, uniq 등의 함수는 SQL 구분을 쏙 빼닮았다
- 쿼리 언어를 구사하듯 개발하는 것과 함수형 프로그래밍에서 배열에 연산을 적용하는 것은 일맥상통하다. 함수형 프로그래밍은 흔히 사용되는 어휘집이나 대수학 개념을 활용해서 데이터 자체의 성격과 구조 체계를 더 깊이 추론할 수 있게 도움을 준다

  ```sql
  SELECT p.firstname FROM Person p
  WHERE p.birthYear > 1903 and p.country IS NOT 'US'
  GROUP BY p.firstname
  ```

- 로대시JS가 지원하는 **믹스인** 기능을 응용하면, 핵심 라이브러리에 함수를 추가하여 확장한 후, 마치 원래 있던 함수처럼 체이닝할 수 있다

예 1: 자바스크립트를 SQL 비슷하게 작성하기

```javascript
_.from(persons)
  .where((p) => p.birthYear > 1900 && p.address.country !== "US")
  .sortBy(["firstname"])
  .select((p) => p.firstname)
  .value();
```

- 자바스크립트 코드도 SQL 처럼 데이터를 함수 형태로 모형화할 수 있는데, 이를 **데이터로서의 함수**라는 개념으로 부르기도 한다

## **3.5 재귀적 사고방식**

- 좀처럼 머릿속에 해법이 떠오리지 않는 어렵고 복잡한 문제를 만나면 문제를 분해할 방법을 찾아야 한다. 전체 문제를 더 작은 분신들로 쪼갤 수 있다면, 작은 문제들을 하나씩 풀면서 전체 문제도 풀 수 있다
- 자바스크립트에서도 XML 파일, HTML 문서, 그래프 등을 파싱할 때 재귀를 다양하게 활용한다

### **3.5.1 재귀란?**

- **재귀**는 주어진 문제를 자기 반복적인 문제들로 잘게 분해한 다음, 이들을 다시 조합해 원래 문제의 정답을 찾는 기법이다
- 재귀 함수의 주된 구성 요소
  1. 기저 케이스(종료조건 이라고도 한다)
  2. 재귀 케이스
- 기저 케이스는 재귀 함수가 구체적인 결과값을 바로 계산할 수 있는 입력 집합이다. 재귀 케이스는 함수가 자신을 호출할 때 전달한 입력 집합을 처맇나다
- 함수가 반복될 수록 입력 집합은 무조건 작아지며, 제일 자미작에 기저 케이스로 빠지면 하나의 값으로 귀결된다

### **3.5.2 재귀적으로 생각하기**

- 재귀는 간단히 이해할 수 있는 개념이 아니다
- 재귀적 사고란, 자기 자신 또는 그 자신을 변형한 버전을 생각하는 것이다
- 예를 들어 \_.reduce 함수를 쓰면 루프는 물론 리스트 크기조차 신경 쓸 필요가 없다. 첫 번째 원소를 나머지 원소드로가 순차적으로 더해가며 결괏값을 계산하는 재귀적 사요방식을 적용하는 셈이다
- 재귀는 변이가 없으므로, 더 강력하고 우수하며 표현적인 방식으로 반복을 대체할 수 있다. 사실상 순수 함수형 언어는 모든 루프를 재귀로 수행하기 때문에 do, for, while 같은 기본 루프 체계조차 없으며, 재귀를 적용한 코드가 더 이해하기 쉽다
- 예 1: 재귀적 덧셈

  ```javascript
  function sum(arr) {
    if (_.isEmpty(arr)) {
      return0;
    }
    return _.first(arr) + sum(_.rest(arr));
  }
  sum([]); // 0
  sum([1, 2, 3, 4, 5, 6, 7, 8, 9]); //45
  ```

  - 더하려는 배열이 빈 배열일 경우는 기저 케이스로서 이때는 당연히 0을 반환한다
  - 이외의 원소가 포함된 배열은 첫 번째 원소를 추출한 후 두 번째 이후 원소들과 계속 재귀적으로 더한다
  - 이때 내부적으로는 재귀 호출 스텍이 겹겹이 쌓인다. 알고리즘이 종료 조건에 이르면 쌓은 스택이 런타임에 의해 즉시 풀리면서 반환문이 모두 실행되고 이 과정에서 실제 덧셈이 이루어진다
  - ES6부터는 꼬리 호출 최적화까지 추가되어 사실상 재귀와 수동 반복의 성능 차이는 미미해졌다

### **3.5.3 재귀적으로 정의한 자료구조**

- 트리는 XML 문서, 파일 시스템, 분류학, 범주, 메뉴 위젯, 소셜 그래프 등 다양한 분야에 쓰이는 아주 일반적인 자료구로자서 처리방법을 알아둘 필요가 있다
- 배열처럼 평탄한 자료구조를 파싱할 때 쓰는 함수형 기법은 이런 트리 구조의 데이터에는 적절하지 않다. 자바스크립트는 언어 자체로 내장 트리 객체를 지원하지는 않으므로 노드 기반의 단순한 자료구조를 만들어야 한다. 노드는 값을 지닌 객체로 자신의 부모와 자식 배열을 래퍼런스로 참조한다

- 예 1: Node 객체

  ```javascript
  class Node{
    constructor(val) {
      this._val = val
      this._parent = parent
      this._children = childred
    }

    isRoot() {
      return isValid(this._parent)
    }
    get childred() {
      return this._childred
    }
    hasChildred() {
      return this._children.length > 0
    }
    get value() {
      return this._val
    }
    set value(val) P
    this._val = bal
  }
  append(child) {
    child._parent = this
    this._children.push(child)
    return this
  }
  toString() {
    return `Node (val: ${this._val}, children: ${this._children.length})`
  }

  //노드는 이렇게 생성한다
  const church = new Node(new Person('Allonzo', 'Church', '111-11-1111'))

  ```

- **트리**는 루트 노드가 포함된 재귀적인 자료구조다

  ```javascript
  class Tree {
    constructor(root) {
      this._root = root;
    }
    static map(node, fn, tree = null) {
      node.value = fn(node.value);
      if (tree === null) {
        tree = new Tree(node);
      }
      if (node.hasChildren) {
        _.map(node, children, function (child) {
          Tree, map(child, fn, tree);
        });
      }
      return tree;
    }
    get root() {
      return this._root;
    }
  }

  //이런식으로 루트부터 시작해 다른 자식 노드들과 연결하면 트리가 완성된다
  church.append(rosser).append(turing).append(kleene);
  kleene.appned(nelson).append(constable);
  rosser.append(mendelson).append(sacks);
  turing.append(gandy);
  ```

  - 노드의 메인 로직은 append 메서드에 있다. 한 노드에 자식을 덧붙일 때 그 자식 노드의 부모 레퍼런스가 이 노드를 가리키게 하고 이 자식 노드를 자식 리스트에 추가한다
  - 각 노드는 Person 객체를 감싼다. 재귀 알고리즘은 루트서부터 모든 자식 모드를 타고 내려가면서 전체 트리를 전위 순환한다. 자기 반복적인 재귀 특성 때문에 순회를 루프에서 시작하든, 임의의 노드에서 시작하든 똑같다
  - 루트 노드에서 출발한 전위순회는 다음 과정을 거친다
    1. 루트 원소의 데이터를 표시한다
    2. 전위 함수를 재귀 호출하여 왼쪽 하위 트리를 탐색한다
    3. 같은 방법으로 오른쪽 하위 트리를 탐색한다
  - Tree.map 함수는 루트 노드 및 각 노드 값을 반환하는 이터레이터 함수를 필수로 받는다

- 변이 및 부수효과 없는 자료형을 다룰 때 데이터 자체를 캡슐화하여 데이터에 접근하는 방법을 통제하는 것이 함수형 프로그래밍의 관점이다
- 함수형 프로그래밍은 원하는 결과를 얻기 위한 비즈니스 로직이 담겨있는 고수준의 연산을 일련의 단계들로 체이닝하는, 간결한 흐름 중심의 모델을 선호한다
- 흐름 중심으로 코딩하면 재사용성, 모듈화 측명에서도 당연히 유리하다
