# Chapter 3 다트 비동기 프로그래밍

## 1. 동기 vs 비동기 프로그래밍

다트 언어는 동기/비동기 프로그래밍을 지원한다.  
동기는 요청하고 나서 응답이 올 때까지 더는 코드를 진행하지 못하고 기다렸다가 응답을 받으면 그제서야 다음 코드를 진행한다.  
반면에 비동기는 요청하고 나서 응답을 받지 않았는데도 대기하지 않고 다음 코드를 진행하고 언제든 답이 오면 그때 응답을 처리하게 된다. 그렇기 때문에 비동기 프로그래밍은 컴퓨터 자원을 낭비하지 않고 더욱 효율적으로 코드를 실행할 수 있다.  
예를 들어 데이터베이스에서 게시판 글을 가져오는 작업이나 이미지 인코딩 등 시간이 걸리는 작업을 동기로 실행하면 앱이 매우 느려질 수 있다. 그렇기 때문에 이런 작업은 비동기로 처리해야 한다.

## 2. Future

Future 클래스는 '미래'라는 단어의 의미대로 미래에 받아올 값을 뜻한다. List나 Set처럼 제네릭으로 어떤 미래의 값을 받아올지를 정할 수 있다.

```dart
Future<String> name; // 미래에 받을 String 값
Future<int> number; // 미래에 받을 int 값
Future<bool> isOpend; // 미래에 받을 boolean 값
```

비동기 프로그래밍은 서버 요청과 같이 오래 걸리는 작업을 기다린 후 값을 받아와야 하기 때문에 미래값을 표현하는 Future 클래스가 필요하다.

```dart
void main() {
  print('시작');
  // Future.delayed()를 사용하면 일정 시간 후에 콜백 함수 실행
  Future.delayed(Duration(seconds: 3), () {
    print('1 + 1 = ${1 + 1}');
  });
  print('종료');
}
```

위와 같은 코드를 실행하면 결과는  
"시작" -> "종료" -> "1 + 1 = 2" 순으로, 코드 작성 순서와 다르게 출력된다.  
Future.delayed()는 비동기 연산이기 때문에 CPU가 3초간 대기해야 한다는 메시지를 받으면 리소스를 허비하지 않고 다음 코드를 바로 실행하기 때문이다.  
이러한 비동기 처리를 하지 않았다면 CPU는 아무것도 하지 않으며 3초를 낭비하였을 것이다. 하지만 비동기 처리를 통해 3초동안 다른 작업을 할 수 있었다.  
이렇게 비동기 처리를 통해 CPU 리소스를 효율적으로 사용할 수 있다.

## 3. async와 await

비동기 처리를 통해 CPU 리소스를 효율적으로 사용할 수 있게 되었지만, 코드가 작성된 순서대로 실행되지 않는다면 프로그래머 입장에서 헷갈릴 수 있다.  
이때 async와 await 키워드를 사용하면 비동기 프로그래밍을 유지하면서도 코드 가독성을 유지할 수 있다.

```dart
// async 키워드는 매개변수 정의와 바디 사이에 입력한다.
Future<void> addNumbers(int number1, int number2) async {
  print('계산 시작');

  // await는 대기하고 싶은 비동기 함수 앞에 입력한다.
  await Future.delayed(Duration(seconds: 3), () {
    print('$number1 + $number2 = ${number1 + number2}');
  });

  print('계산 끝');
}

void main() {
  addNumbers(1, 2);
}
```

await 키워드는 async로 지정된 함수 내에서만 사용할 수 있다.  
위와 같이 함수를 async로 지정해주고 나서 대기하고 싶은 비동기 함수를 실행할 때 await 키워드를 사용해주면  
"계산 시작" -> "1 + 2 = 3" -> "계산 끝" 와 같이 코드를 작성한 순서대로 실행된다.  
await 키워드를 통해 Future.delayed() 함수가 실행완료 될 때까지 다음 코드를 실행하지 않는 것이다.
물론 이 경우에도 비동기 프로그래밍 특성을 그대로 유지하고 있다.

```dart
void main() {
  addNumbers(1, 2);
  addNumbers(3, 4);
}
```

메인 함수만 위와 같이 변경하고 실행하면 결과는  
"계산 시작" -> "계산 시작"  
-> "1 + 2 = 3" -> "계산 끝"  
-> "3 + 4 = 7" -> "계산 끝"  
와 같이 출력된다.  
addNumbers() 함수가 비동기 프로그래밍으로 실행되었기 때문에 addNumbers(1,2)가 끝나기 전에 addNumbers(3,4)가 실행된 것이다.  
addNumbers(1,2) 함수가 실행되고, "계산 시작"을 출력한 뒤, Future.delayed()가 실행되며 3초를 기다려야 할 때, CPU의 리소스가 낭비되지 않고, 바로 다음 실행할 코드인 addNumbers(3,4)를 실행한 것이다.  
그 후, addNumbers(3,4) 내에서 다시 "계산 시작"을 출력하고
Future.delayed()가 실행된다.  
그리고 3초 경과 뒤, (1,2)의 계산 결과 와 "계산 끝"이 출력되고 (3,4)의 계산 결과와 "계산 끝"이 한번 더 출력된다.

```dart
void main() async {
  await addNumbers(1, 2);
  await addNumbers(3, 4);
}
```

만약 위와 같이 main 함수에 async를 적용하고, addNumbers() 함수에 await을 적용한다면 addNumbers() 함수도 순차적으로 실행될 것이다. 즉, 결과는  
"계산 시작" -> "1 + 2 = 3" -> "계산 끝"  
"계산 시작" -> "3 + 4 = 7" -> "계산 끝"  
와 같이 순차적으로 출력된다.

### 3.1 결괏값 반환받기

위에서는 Future<void> 타입 함수를 실행하여, 함수 내에서 결과를 직접 출력하였지만, async와 await 키워드를 사용한 함수에서도 결괏값을 받아 낼 수 있다.

```dart
Future<int> addNumbers(int number1, int number2) async {
  print('계산 시작');

  await Future.delayed(Duration(seconds: 3), () {
    print('$number1 + $number2 = ${number1 + number2}');
  });

  print('계산 끝');

  return number1 + number2;
}

void main() async {
  // 일반 함수와 동일하게 반환값을 받을 수 있다.
  final ret = await addNumbers(1, 2);
  print('결괏값 $ret');
}
```

위와 같이 Future<int> 타입을 반환하는 addNumbers 함수를 통해 int 값을 반환 받을 수 있다.
결과는 아래와 같다.

```
계산 시작
1 + 2 = 3
계산 끝
결괏값 3
```

## 4. Stream

Future는 반환값을 딱 한 번 받아내는 비동기 프로그래밍에 사용한다.  
지속적으로 값을 반환 받을 때는 Stream을 사용한다.
Stream은 한 번 listen하면 Stream에 주입되는 모든 값들을 지속적으로 받아온다.

### 4.1 Stream 기본 사용법

Stream을 사용하려면 플러터에서 기본으로 제공하는 dart:async 패키지를 불러와야 한다.  
그다음 패키지에서 제공하는 StreamController를 listen() 해야 값을 지속적으로 반환받을 수 있다.

```dart
// dart:async 패키지를 불러온다.
import 'dart:async';

void main() async {
  final controller = StreamController(); //StreamController 선언
  final stream = controller.stream; // Stream 가져오기

  // Stream에 listen() 함수를 실행하면 값이 주입될 때마다
  // 콜백 함수를 실행할 수 있다.
  final streamListener1 = stream.listen((val) {
    print(val);
  });

  // Stream에 값 주입
  controller.sink.add(1);
  controller.sink.add(2);
  controller.sink.add(3);
  controller.sink.add(4);
  controller.sink.add(5);
}
```

아래는 출력된 결과이다.

```
1
2
3
4
5
```

위와 같이 Stream을 통해 값을 지속적으로 반환받을 수 있다.

## 4.2 브로드캐스트 스트림

Stream은 단 한번만 listen()을 수행할 수 있다.
하지만 브로드캐스트 스트림을 사용하면 Stream을 여러번 listen()하도록 변환할 수 있다.

```dart
// dart:async 패키지를 불러온다.
import 'dart:async';

void main() async {
  final controller = StreamController();
  // 여러 번 리슨할 수 있는 Broadcast Stream 객체 생성
  final stream = controller.stream.asBroadcastStream();

  // 첫 listen() 함수
  final streamListener1 = stream.listen((val) {
    print('listening 1');
    print(val);
  });

  // 두 번째 listen() 함수
  final streamListener2 = stream.listen((val) {
    print('listening 2');
    print(val);
  });

  // add()를 실행할 때마다
  // listen()하는 모든 콜백 함수에 값이 주입된다.
  controller.sink.add(1);
  controller.sink.add(2);
  controller.sink.add(3);
}
```

아래는 실행 결과이다.

```
listening 1
1
listening 2
1
listening 1
2
listening 2
2
listening 1
3
listening 2
3
```

이렇게 브로드캐스트 스트림을 사용해 하나의 스트림을 생성하고 여러 개의 listen() 함수를 실행할 수 있다.

## 4.3 함수로 Stream 반환하기

StreamController를 직접 사용하지 않고 Stream을 반환하는 함수를 작성할 수 있다.  
Stream을 반환하는 함수는 async\*로 함수를 선언하고 yield 키워드로 값을 반환해주면 된다.

```dart
import 'dart:async';

// Stream을 반환하는 함수는 async*로 선언
Stream<String> calculate(int number) async* {
  for (int i = 0; i < 5; i++) {
    // StreamController의 add()처럼 yield 키워드를 이용해서 값 반환
    yield 'i = ${i * number}';
    await Future.delayed(Duration(seconds: 1));
  }
}

void playStream() {
  // StreamController와 마찬가지로 listen() 함수로 콜백 함수 입력
  calculate(2).listen((val) {
    print(val);
  });
}

void main() {
  playStream();
}
```

위와 같이 함수로 Stream을 반환받을 수 있다.  
결과값은 1초 간격으로 아래와 같이 출력된다.

```
i = 0
i = 2
i = 4
i = 6
i = 8
```

# Chapter 5 기본 위젯 알아보기

플러터는 화면에 보여지는 UI와 관련된 모든 요소가 위젯으로 구성되어 있다. 플러터는 프레임워크는 수십가지 기본 위젯을 제공하며 앱 개발자가 직접 위젯을 만들 수도 있다.

위젯은 자식을 하나만 갖는 위젯과 자식을 여럿 갖는 위젯으로 나뉜다.

자식을 하나만 갖는 대표적인 위젯들은 아래와 같으며 대체로 child 매개변수를 입력받는다.

- Container 위젯 : 자식을 담는 컨테이너 역할을 한다. 배경색, 너비와 높이, 테두리 등의 디자인 지정 가능
- GestureDetector 위젯 : 제스처 기능을 자식 위젯에서 인식하는 위젯이다. 탭, 드래그, 더블 클릭 같은 제스처 기능이 자식 위젯에 인식됐을 때 함수를 실행할 수 있다.
- SizedBox 위젯 : 높이와 너비를 지정하는 위젯. Container 위젯과 다르게 디자인적 요소는 적용할 수 없고 const 생성자로 선언할 수 있어서 퍼포먼스 측면에서 더 효율적이다.

다수의 자식을 입력할 수 있는 위젯은 children 매개변수를 입력받으며 리스트로 여러 위젯을 입력할 수 있다.
대표적인 다수의 위젯은 아래와 같다.

- Column 위젯 : children 매개변수에 입력된 모든 위젯들을 세로로 배치한다.
- Row 위젯 : children 매개변수에 입력된 모든 위젯들을 가로로 배치한다.
- ListView 위젯 : 리스트를 구현할 때 사용한다. children 매개변수에 다수의 위젯을 입력할 수 있고 입력된 위젯이 화면을 벗어나면 스크롤이 가능해진다.

## 1. Children과 Child의 차이점

플러터는 위젯 아래에 계속 위젯이 입력되는 형태로 '위젯 트리'를 구성하여 UI를 제작한다.
child는 위젯을 하나만 추가할 수 있고 children은 여럿을 추가할 수 있다.

## 2. 위젯 실습용 템플릿 작성

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Center(
          child: // 여기에 예제 코드 작성
        ),
      ),
    );
  }
}
```

위와 같이 기본 코드를 구현한 후 child 매개변수에 실습 예제들을 작성하면 된다.

## 3 텍스트 관련 위젯

Text 위젯은 글자를 적고 스타일링하는 위젯이다.
첫 번째 포지셔널 파라미터에 원하는 문자열을 작성하고
style 이라는 네임드 파라미터를 사용해 스타일을 작성한다.

```dart
Text(
  '텍스트 위젯',
  // 글자에 스타일 적용
  style: TextStyle(
    // 글자 크기
    fontSize: 16.0,
    // 글자 굵기
    fontWeight: FontWeight.w700,
    // 글자 색상
    color: Colors.blue,
  ),
),
```

## 4 제스처 관련 위젯

제스처 관련 위젯은 하위 위젯에 탭이나 드래그처럼 특정 제스처가 입력됐을 때 인지하고 콜백 함수를 실행한다.

### 4.1 Button 위젯

플러터 머터리얼 패키지에서 기본 제공하는 버튼으로
TextButton, OutlinedButto, ElevatedButton이 있다.

```dart
TextButton(
  // 클릭 시 실행할 함수
  onPressed: () {},
  // 스타일 지정
  style: TextButton.styleFrom(
    // 주 색상 지정
    foregroundColor: Colors.red,
  ),
  // 버튼에 넣을 위젯
  child: Text('텍스트 버튼'),
)
```

위와 같은 방식으로 구현한다.

### 4.2 IconButton 위젯

IconButton은 아이콘을 버튼으로 생성하는 위젯이다.
onPressed 매개변수에 IconButton을 누르면 실행할 콜백 함수를 제공할 수 있다. 또한 Icons 클래스를 통해 플러터에서 제공하는 기본 아이콘들을 사용할 수 있다.

```dart
IconButton(
  onPressed: () {},
  icon: Icon(
    // 플러터에서 기본으로 제공하는 아이콘
    Icons.home,
  ),
)
```

### 4.3 GestureDetector 위젯

GestureDetecor는 손가락으로 하는 여러 가지 입력을 인지하는 위젯이다.

```dart
GestureDetector(
  // 한 번 탭했을 때 실행할 함수
  onTap: () {
    print('on tap');
  },
  // 두 번 탭했을 때 실행할 함수
  onDoubleTap: () {
    print('on double tap');
  },
  // 길게 눌렀을 때 실행할 함수
  onLongPress: () {
    print('on long press');
  },
  // Gesture를 적용할 위젯
  child: Container(
    decoration: BoxDecoration(
      color: Colors.red,
    ),
    width: 100.0,
    height: 100.0,
  ),
)
```

위와 같은 방식으로 구현할 수 있다.

### 4.4 FloatingActionButton 위젯

FloatingActionButton은 Matrerial Design에서 추구하는 버튼 형태이다. 안드로이드 앱들을 사용하다 보면 화면의 오른쪽 아래에 동그란 플로팅 작업 버튼을 쉽게 볼 수 있다.
이를 FloatingActionButton을 통해 구현할 수 있다.

## 5. 디자인 관련 위젯

### 5.1 Container 위젯

Container 위젯은 말 그대로 다른 위젯을 담는 데 사용된다.
위젯의 너비와 높이를 지정하거나, 배경이나 테두리를 추가할 때 많이 사용된다.  
Container 위젯은 child 매개변수를 사용할 수 있다.

### 5.2 SizedBox 위젯

SizedBox 위젯은 일반적으로 일정 크기의 공백을 두고 싶을 때 사용된다.  
Container 위젯으로도 공백을 만들 수 있지만 SizedBox는 const 생성자를 사용했을 때 퍼포먼스에서 이점을 얻을 수 있다.

### 5.3 Padding 위젯

Padding 위젯은 child 위젯에 여백을 제공할 때 사용한다.

```dart
Padding(
  // 상하, 좌우로 모두 16픽셀만큼 패딩을 적용한다.
  padding: EdgeInsets.all(
    16.0,
  ),
  child: Container(
    color: Colors.red,
    width: 50.0,
    height: 50.0,
  ),
)
```

위와 같이 child 위젯에 여백을 제공할 수 있다.

### 5.4 SafeArea

현대 핸드폰은 크기도 여러 가지고 디자인도 여러 가지이다.  
플러터는 가용되는 화면을 모두 사용한다.  
때문에 노치가 있는 핸드폰에서 노치에 위젯들이 가릴 수 있다.  
이때, SafeArea 위젯을 사용하면 따로 기기별로 예외 처리를 하지 않고도 안전한 화면에서만 위젯을 그릴 수 있다.

## 6. 배치 관련 위젯

Row, Column, Flexible, Expanded, Stack 위젯 등이 존재한다.

- Row 위젯은 가로로 위젯을 배치할 때 사용된다.
- Column 위젯은 세로로 위젯을 배치할 때 사용된다.
- Flexible 위젯은 Row나 Column에서 하위 위젯이 비율만큼 공간을 차지할 수 있게 해준다.
- Expanded 위젯은 Row나 Column에서 하위 위젯이 최대한의 공간을 차지할 수 있게 해준다.
- Stack 위젯은 하위 위젯들을 순서대로 겹쳐준다. 위젯 위에 위젯을 올린 듯한 효과를 줄 수 있다.
