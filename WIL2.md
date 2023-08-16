# 다트 객체지향 프로그래밍

다트 언어는 높은 완성도로 객체지향 프로그래밍을 지원한다. 플러터 역시 객체지향 프로그래밍 중심으로 설계된 언어이다.

## 1. 클래스

클래스는 일종의 설계도로서 데이터가 보유할 속성과 기능을 정의하는 자료구조이다.

```dart
// class 키워드를 입력 후 클래스명을 지정해 클래스를 선언
class Human {
  // 클래스에 종속되는 변수를 지정할 수 있다.
  String name = '고길동';

  // 클래스에 종속되는 함수를 지정할 수 있다.
  // 클래스에 종속되는 함수를 메서드라고 한다.
  void sayName() {
    print('저는 ${this.name}입니다.');
    // 클래스 내부의 속성을 지칭하고 싶을 때는 this 키워드를 사용
    // this.name 은 현재 클래스의 name 변수를 지칭한다.
    print('저는 $name입니다.');
    // 스코프 안에 같은 속성 이름이 하나만 존재하면 this를 생략할 수 있다.
  }

  void sayFriendName(String name) {
    print('저는 ${this.name}입니다.');
    // 이 경우 sayName2 함수가 name이라는 매개변수를 통해 문자열을 받아온다.
    // 따라서 Human 클래스 내부의 속성인 name을 지칭하려면 무조건 this.name을 사용해야 한다.
    print('제 친구는 ${name}입니다.');
    // 그냥 name을 사용할 경우, sayName2 함수의 매개변수로 받은 문자열을 지칭하게 된다.
  }
}
```

클래스를 사용하려면 인스턴스를 생성해야 한다.  
인스턴스란, 클래스를 이용해서 선언한 객체를 의미한다.  
즉, 클래스를 설계도, 인스턴스화를 시킨 것이 실물 아파트라고 생각하면 된다.

```dart
void main() {
  // 변수 타입을 Human으로 지정하고 Human의 인스턴스를 생성할 수 있다.
  // '클래스()' 와 같은 방법으로 생성한다. new는 생략할 수 있다.
  Human ko = Human();
  // Human ko = new Human(); // new를 사용해도 되고 생략해도 됨
  ko.sayName();
  // '저는 고길동입니다.' 가 2번 출력된다.
  ko.sayFriendName('마이콜');
  // '저는 고길동입니다.' 와 '제 친구는 마이콜입니다.' 가 출력된다.
}
```

### 1.1 생성자

생성자는 클래스의 인스턴스를 생성하는 메서드이다.  
클래스와 같은 이름이어야 하며, 함수에 매개변수를 선언하는 것처럼 매개변수를 지정해줄 수 있다.

```dart
class Human {
  // 생성자에서 입력받는 변수들은 일반적으로 final 키워드 사용
  // 인스턴스화한 다음에 변수의 값을 변경하는 실수를 막기 위함임
  final String name;

  // ':' 기호 뒤에 입력받은 매개변수가 지정될 클래스 변수를 지정해준다.
  Human(String name) : this.name = name;

  // 아래와 같은 생성자는 매개변수를 변수에 저장하는 과정을 생략한다.
  // Human(this.name); // 이 경우, 매개변수로 입력받은 값을 해당되는 변수에 자동으로 저장한다.

  void sayName() {
    print('저는 ${this.name}입니다.');
  }
}

void main() {
  Human ko = Human('고길동');
  ko.sayName();
  // '저는 고길동입니다.' 출력

  Human call = Human('마이콜');
  call.sayName();
  // '저는 마이콜입니다.' 출력
}
```

### 1.2 네임드 생성자

네임드 클래스는 네임드 파라미터와 상당히 비슷한 개념이다. 일반적으로 클래스를 생성하는 여러 방법을 명시하고 싶을 때 사용한다.  
사용자는 클래스를 생성할 때, 여러 생성자 중 하나를 선택해서 사용할 수 있다.

```dart
class Human {
  final String name;
  final int familyCount;

  Human(String name, int familyCount)
      : this.name = name,
        this.familyCount = familyCount;

  // 네임드 생성자
  // {클래스명.네임드 생성자명} 형식, 나머지 과정은 기본 생성자와 같다.
  Human.fromMap(Map<String, dynamic> map)
      : this.name = map['name'],
        this.familyCount = map['familyCount'];

  void sayName() {
    print('저는 ${this.name}입니다. 저는 ${this.familyCount}인 가족입니다.');
  }
}

void main() {
  Human ko = Human('고길동', 8);
  ko.sayName();
  // '저는 고길동입니다. 저는 8인 가족입니다.' 출력

  Human call = Human.fromMap({'name': '마이콜', 'familyCount': 1});
  call.sayName();
  // '저는 마이콜입니다. 저는 1인 가족입니다.' 출력
}
```

### 1.3 프라이빗 변수

일반적으로 프라이빗 변수는 클래스 내부에서만 사용하는 변수를 칭하지만, 다트 언어에서는 **같은 파일에서만** 접근 가능한 변수이다.

```dart
class Human {
  // '_'로 변수명을 시작하면
  // 프라이빗 변수를 선언할 수 있다.
  String _name;

  Human(this._name);
}

void main() {
  Human ko = Human('고길동');

  // 같은 파일에서는 _name 변수에 접근 가능
  // 다른 파일에서는 _name 변수에 접근 불가능
  print(ko._name);
}
```

### 1.4 게터/세터

getter는 말 그대로 값을 가져올 때 사용되고 setter는 값을 지정할 때 사용된다.  
가변 변수를 선언해도 값을 가져오고, 지정할 수 있지만 게터와 세터를 사용하면 어떤 값이 노출되고 어떤 형태로 노출될지 그리고 어떤 변수를 변경 가능하게 할지 유연하게 정할 수 있다.

```dart
class Human {
  String _name = '고길동';

  // get 키워드를 사용해서 게터임을 명시함
  // 게터는 매개변수를 전혀 받지 않음
  String get name {
    return this._name;
  }

  // 세터는 set 키워드를 사용해서 선언
  // 세터는 매개변수로 딱 하나의 변수를 받을 수 있음
  set name(String name) {
    this._name = name;
  }
}

void main() {
  Human ko = Human();

  ko.name = '희동이'; // 세터
  print(ko.name); // 게터
  // 게터와 세터는 모두 변수처럼 사용한다.
  // 즉, 사용할때 메서드명 뒤에 ()를 붙이지 않는다.
}
```

최근에는 객체지향 프로그래밍을 할 때 변수의 값을 불변성 특성으로 사용하기 때문에 세터는 거의 사용하지 않는다. 하지만 게터는 종종 사용한다.

## 2. 상속

extends 키워드를 사용해 상속할 수 있다.  
상속은 어떤 클래스의 기능을 다른 클래스가 사용할 수 있게 하는 기법이다. 기능을 물려주는 클래스를 부모 클래스, 물려받는 클래스를 자식 클래스라고 한다.  
부모 클래스에 공통으로 사용하는 변수와 메서드를 정의해 상속받으면 자식 클래스들은 해당 값들을 사용할 수 있어서 중복 코딩하지 않아도 된다.

```dart
class Human {
  final String name;
  final int familyCount;

  Human(this.name, this.familyCount);

  void sayName() {
    print('저는 ${this.name}입니다.');
  }

  void sayFamilyCount() {
    print('저는 ${this.familyCount}인 가족입니다.');
  }
}
```

위의 Human 클래스를 상속하는 Man 클래스를 아래와 같이 만들 수 있다.

```dart
class Human {
  final String name;
  final int familyCount;

  Human(this.name, this.familyCount);

  void sayName() {
    print('저는 ${this.name}입니다.');
  }

  void sayFamilyCount() {
    print('저는 ${this.familyCount}인 가족입니다.');
  }
}

class Man extends Human {
  // super는 부모 클래스르 지칭한다.
  // super를 통해 부모 클래스의 생성자를 실행하여 멤버변수 값 설정
  Man(String name, int familyCount) : super(name, familyCount);

  // 상속받지 않은 기능
  void sayMale() {
    print('저는 남자 입니다.');
  }
}

void main() {
  Man ko = Man('고길동', 8);

  ko.sayName(); // 부모에게 물려받은 메서드
  ko.sayFamilyCount(); // 부모에게 물려받은 메서드
  ko.sayMale(); // 자식이 새로 추가한 메서드

  // 출력 결과
  // 저는 고길동입니다.
  // 저는 8인 가족입니다.
  // 저는 남자 입니다.
}
```

{class 자식클래스 extends 부모클래스} 형식으로 상속을 받는다. 자식 클래스는 부모 클래스의 모든 기능을 상속받는다.  
상속받지 않은 메서드나 변수를 추가할 수도 있다.  
사용법은 부모 클래스와 같다.

## 3. 오버라이드

오버라이드는 부모 클래스 또는 인터페이스에 정의된 메서드를 재정의할 때 사용된다.  
다트에서는 override 키워드를 생략할 수 있다.  
한 클래스에 이름이 같은 메서드가 존재할 수 없기 때문에 부모 클래스나 인터페이스에 이미 존재하는 메서드명을 입력하면 override 키워드를 생략해도 메서드가 덮어써진다.  
하지만 직접 명시하는 게 협업 및 유지보수에 유리하다.

```dart
class Human {
  final String name;
  final int familyCount;

  Human(this.name, this.familyCount);

  void sayName() {
    print('저는 ${this.name}입니다.');
  }

  void sayFamilyCount() {
    print('저는 ${this.familyCount}인 가족입니다.');
  }
}

class Woman extends Human {
  // super.변수 형식으로 매개변수를 받게 되면
  // 해당되는 변수에 자동으로 매개변수가 저장된다.
  Woman(super.name, super.familyCount);

  @override
  void sayName() {
    print('저는 ${this.name}이고, 여자 입니다.');
  }
}

void main() {
  Woman ko = Woman('고영희', 8);

  // 자식 클래스의 오버라이드된 메서드 사용
  ko.sayName();

  // sayFamilyCount는 오버라이드하지 않았기 때문에
  // 그대로 Human 클래스의 메서드가 실행된다.
  ko.sayFamilyCount();
}
```

## 4. 인터페이스

상속은 공유되는 기능을 상속받는 개념이지만 인터페이스는 공통으로 필요한 기능을 정의만 해두는 역할을 한다.  
다트에는 인터페이스를 지정하는 키워드가 따로 없다.  
implements 키워드를 사용하여 원하는 클래스를 인터페이스로 사용할 수 있다. 이 때, 인터페이스로 사용할 클래스에 있는 모든 기능을 다시 재정의해 주어야만 한다.  
클래스의 상속은 다중상속이 불가능하지만 인터페이스는 적용 개수에 제한이 없다. , 기호를 사용해 인터페이스를 나열해 여러 인터페이스를 적용해 줄 수 있다.

```dart
class Human {
  final String name;
  final int familyCount;

  Human(this.name, this.familyCount);

  void sayName() {
    print('저는 ${this.name}입니다.');
  }

  void sayFamilyCount() {
    print('저는 ${this.familyCount}인 가족입니다.');
  }
}

class Woman implements Human {
  final String name;
  final int familyCount;

  Woman(this.name, this.familyCount);

  void sayName() {
    print('제 이름은 ${this.name}이고 여자 입니다.');
  }

  void sayFamilyCount() {
    print('저는 ${this.familyCount}인 가족입니다.');
  }
}
```

위와 같이 Woman 클래스는 Human 클래스를 인터페이스로 사용했으므로 Woman 클래스 내에서 Human 클래스에서 정의한 모든 기능을 다시 정의해주어야 했다.  
위와 같이 기능의 재정의를 강제하기 위해 사용하는 것이 인터페이스이다.  
이를 통해, 특정 기능을 실수로 빼먹는 일 등을 방지할 수 있다.

## 5. 믹스인

믹스인은 특정 클래스에 원하는 기능들만 골라넣을 수 있는 기능이다. 또한, 인터페이스처럼 한 개의 클래스에 여러 개의 믹스인을 적용할 수도 있다.

```dart
class Human {
  final String name;

  Human(this.name);

  void sayName() {
    print('저는 ${this.name}입니다.');
  }
}

// on 키워드를 사용하여 특정 클래스에만 mixin 적용
mixin LeftHanded on Human {
  void islefthander() {
    // Human 클래스에 적용되는 mixin 이므로
    // Human 클래스의 멤버 변수인 name을 사용할 수 있다.
    print('${this.name}은 왼손잡이 입니다.');
  }
}

// 믹스인을 적용할 때, with 키워드를 사용한다.
class LeftHander extends Human with LeftHanded {
  LeftHander(super.name);
}

void main() {
  LeftHander lee = LeftHander('이적');

  // 믹스인에 정의된 함수 사용 가능
  lee.islefthander();

  // '이적은 왼손잡이 입니다.' 출력
}
```

Human 클래스의 기능을 가지고 있고, 추가로 왼손잡이의 기능을 가진 LeftHander 라는 클래스를 만들기 위해  
Human 클래스를 상속시키고, LeftHanded 믹스인을 적용시켰다.  
위와 같이 믹스인을 사용하여, 원하는 기능들을 추가로 적용할 수 있다.  
이 때, LeftHanded 믹스인은 on 키워드를 통해 Human 클래스에만 적용 가능하도록 만들었다.  
on 키워드 없이 만들어진 믹스인은 아무 클래스에나 적용가능하다.

## 6. 추상

추상은 상속이나 인터페이스로 사용하는 데 필요한 속성만 정의하고, 인스턴스화할 수 없도록 만드는 기능이다.  
추상 클래스는 추상 메서드를 선언할 수 있으며, 추상 메서드는 함수의 반환 타입, 이름, 매개변수만 정의하고 함수 바디의 선언을 자식 클래스에서 필수로 정의하도록 강제한다.

즉, 추상 클래스를 통해 필요한 기능들의 틀을 잡아두고, 실질적인 구현은 추상 클래스를 상속받는 클래스에서 이루어진다.  
따라서 추상 클래스에서는 기능들의 구현이 안되어 있으므로 인스턴스화가 불가능하다.

```dart
// abstract 키워드를 사용해 추상 클래스 지정
abstract class Animal {
  final String species;
  final String name;

  Animal(this.species, this.name);

  // 추상 메서드 선언
  void sayName();
}

// implements 키워드를 사용해 추상 클래스를 구현
class Dinosaur implements Animal {
  final String species = '공룡';
  final String name;

  Dinosaur(this.name);

  void sayName() {
    print('저는 ${this.species}이고, 이름은 ${this.name}입니다.');
  }
}

void main() {
  Dinosaur dooly = Dinosaur('둘리');

  dooly.sayName();
  // '저는 공룡이고, 이름은 둘리입니다.' 출력
}
```

위와 같이 추상 메서드는 부모 클래스를 인스턴스화할 일이 없고, 자식 클래스들에 필수적으로 또는 공통적으로 정의돼야 하는 메서드가 존재할 때 사용된다.

## 7. 제네릭

제네릭은 클래스나 함수의 정의를 선언할 때가 아니라 인스턴스화하거나 실행할 때로 미룬다.  
특정 변수의 타입을 하나의 타입으로 제한하고 싶지 않을 때 자주 사용한다.

Map, List, Set 등에서 사용한 <> 사이에 입력되는 값이 제네릭 문자이다.  
예를 들어, List<String>은 String 값으로 구성된 리스트를 생성하겠다는 뜻이 된다.  
즉, 제네릭은 클래스나 함수 내부에서 사용할 데이터 타입을 하나의 타입으로 제한하지 않고, 외부에서 사용자가 지정할 수 있도록 하는 기능이다.  
제네릭을 통해, 코드 재사용성을 높일 수 있다.  
예를 들어, 정수를 더하는 함수 addInt(), 실수를 더하는 함수 addDouble(), 문자열을 더하는 함수 addString() 등 여러개의 함수를 만드는 대신 add() 함수를 제네릭을 사용하여 정의해주는 것이 훨씬 코드가 간결하고, 재사용성이 높다.

```dart
// 인스턴스화할때 입력받을 타입을 T로 지정한다.
// 이때, T는 타입 변수로, 꼭 T로 지정할 필요는 없고
// A나 B등 다른 이름으로 지정해줘도 된다.
class Cache<T> {
  // data의 타입을 추후 외부에서 입력될 T 타입으로 지정한다.
  final T data;

  Cache(this.data);
}

void main() {
  // T의 타입을 List<int>로 입력
  final cache1 = Cache<List<int>>([1, 2, 3]);

  // T의 타입을 List<double>로 입력
  final cache2 = Cache<List<double>>([4.4, 5.5, 6.6]);

  // 제네릭에 입력된 값을 통해 data 변수의 타입이 자동으로 유추된다.
  print(cache1.data.reduce((value, element) => value + element));
  // 6 출력

  print(cache2.data.reduce((value, element) => value + element));
  // 16.5 출력
}
```

## 8. 스태틱

지금까지 작성한 변수와 메서드 등 모든 속성은 각 '클래스의 인스턴스'에 귀속되었다.  
하지만 static 키워드를 사용하면 클래스 자체에 귀속된다.

```dart
class Counter {
  // static 키워드를 사용해서 static 변수 선언
  static int i = 0;

  Counter() {
    // static 변수는 인스턴스가 아닌 클래스에 직접 귀속되기 때문에 this 사용 불가
    // this.i++; // 에러 발생
    i++;
    // Counter.i++; // 클래스명을 통해 접근할 수도 있음
    print(i);
  }
}

void main() {
  Counter count1 = Counter();
  // 생성자를 통해 1 출력
  Counter count2 = Counter();
  // 생성자를 통해 2 출력
  Counter count3 = Counter();
  // 생성자를 통해 3 출력

  // 인스턴스를 통해 클래스의 static 변수에 접근 시 에러 발생
  // print(count1.i); // 에러 발생

  // 클래스의 static 변수에 접근하려면 인스턴스가 아닌 클래스를 통해 접근
  print(Counter.i);
  // 3 출력
}
```

## 9. 캐스케이드 연산자

Cascade Operator는 인스턴스에서 해당 인스턴스의 속성이나 멤버 함수를 연속해서 사용하는 기능이다.  
'..' 기호를 사용한다.

```dart
class Human {
  String name;
  int familyCount;

  Human(this.name, this.familyCount);

  void sayName() {
    print('저는 ${this.name}입니다.');
  }

  void sayFamilyCount() {
    print('저는 ${this.familyCount}인 가족입니다.');
  }
}

void main() {
  // 캐스케이드 연산자 (..)를 사용하여
  // 선언한 변수의 메서드를 연속으로 실행할 수 있다.
  Human man = Human('고길동', 8)
    ..sayName()
    ..sayFamilyCount(); // 제일 마지막에 ';'를 입력해준다.
  // '저는 고길동입니다.', '저는 8인 가족입니다.' 출력

  // 위 처럼 선언과 동시에 사용할 수도 있고
  // 아래 처럼 선언 후에도 사용할 수 있다.
  // 또한 캐스케이드 연산자로 멤버 변수에도 접근할 수 있다.
  man
    ..name = '마이콜'
    ..familyCount = 1
    ..sayName()
    ..sayFamilyCount();
  // '저는 마이콜입니다.', '저는 1인 가족입니다.' 출력
}
```

위와 같이 캐스케이드 연산자를 사용하여 더 간결한 코드를 작성할 수 있다.
