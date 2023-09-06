# WIL5
## Figma를 활용한 UI 구현
그동안 배웠던 개념과 위젯들을 이용해 피그마를 통해 UI를 구현해 보았다.  
UI를 구현하면서 새로 접하게된 위젯들도 많았다.  
```dart
// 앱 하단에 내비게이터 바를 구현하는 위젯
// Scaffold 위젯의 named parameter로 입력
bottomNavigationBar: BottomNavigationBar(
    // 4개 이상의 아이템을 지정해줄 때는 fixed값을 설정해줘야 한다고 한다.
    type: BottomNavigationBarType.fixed,
    // 바텀 내비게이터바의 백그라운드 색깔 지정
    backgroundColor: const Color(0xFFFFFFFF).withOpacity(0.8),
    // 현재 선택된 아이템외의 다른 아이템의 색깔 지정
    unselectedItemColor: const Color(0xFF6C6C6C),
    // 현재 선택된 아이템의 색깔 지정
    selectedItemColor: const Color(0xFF2F2F2F),
    // 0번째 아이템을 선택된 아이템으로 지정
    currentIndex: 0,
    // item으로 Icon 4개를 지정
    items: const [
    BottomNavigationBarItem(
        icon: Icon(
        Icons.home_outlined,
        ),
        label: '홈',
    ),
    BottomNavigationBarItem(
        icon: Icon(
        Icons.calendar_today_outlined,
        ),
        label: '스케줄',
    ),
    BottomNavigationBarItem(
        icon: Icon(
        Icons.chat_outlined,
        ),
        label: '채팅',
    ),
    BottomNavigationBarItem(
        icon: Icon(
        Icons.perm_identity_outlined,
        ),
        label: 'My',
    ),
    ],
),
```
위와 같이 앱 화면 하단에 내비게이터 바를 만들어 주는 위젯에 대해 새로 알게되었다.  

또한 폰트나 이미지파일을 다운로드 받아 적용해주는 방법도 배웠다.
먼저 프로젝트 폴더 내에 assets 폴더를 만들어서 거기에 다운로드 받은 파일을 넣어주고, pubspec.yaml 파일에 파일경로를 입력해준 뒤 사용할 수 있었다.
```yaml
  assets:
    - assets/images/image1.jpeg
    - assets/images/image2.jpeg
    - assets/images/image3.png

  fonts:
    - family: ChangwonDangamAsac
      fonts:
        - asset: assets/fonts/ChangwonDangamAsac-Bold_0712.otf
          weight: 700
    - family: Pretendard
      fonts:
        - asset: assets/fonts/Pretendard-Thin.otf
          weight: 100
        - asset: assets/fonts/Pretendard-ExtraLight.otf
          weight: 200
        - asset: assets/fonts/Pretendard-Light.otf
          weight: 300
        - asset: assets/fonts/Pretendard-Regular.otf
          weight: 400
        - asset: assets/fonts/Pretendard-Medium.otf
          weight: 500
        - asset: assets/fonts/Pretendard-SemiBold.otf
          weight: 600
        - asset: assets/fonts/Pretendard-Bold.otf
          weight: 700
        - asset: assets/fonts/Pretendard-ExtraBold.otf
          weight: 800
        - asset: assets/fonts/Pretendard-Black.otf
          weight: 900
```
위와 같이 pubsec.yaml 파일에 다운받은 파일의 경로를 입력해주고  

폰트의 경우 아래와 같은 방법으로 적용해줄 수 있었다.

```dart
Text(
    '더행사',
    style: TextStyle(
        fontSize: 24,
        fontFamily: 'ChangwonDangamAsac',
        fontWeight: FontWeight.w700,
        color: Color(0xFFF4F4F4),
    ),
),
```

이미지의 경우 아래와 같은 방법으로 적용해 줄 수 있었다.

```dart
Image.asset(
    // 이미지의 경로 입력
    'assets/images/image1',
    // 이미지를 어떠한 형태로 채울지 입력
    // Boxfit.cover의 경우 비율을 유지한채로 지정된 구역을 꽉 채움
    fit: BoxFit.cover,
)
```
위와 같이 경로를 입력해주고, 지정된 영역을 어떤 형태로 채울지 fit 옵션을 지정해주면 된다.  
cover옵션 외에도  
- contain (원본)
- fill (화면꽉채움,비율변경됨)
- fitWidth (너비에 맞게 확대,축소)
- fitHeight (높이에 맞게 확대, 축소)
- none (영역 크기만큼 원본의 가운데를 출력)  

등의 옵션이 있었다.  

또한, 2번째 홈스크린을 구현하면서 Gridview 위젯에 대해서도 알게 되었다.  
Gridview는 자식 위젯을 그리드 형태로 표시해주는 위젯이다.  
사용방법은 아래와 같다.

```dart
// Gridview.count는 한 행에 표시할 자식의 개수를 기준으로 grid를 만드는 위젯
GridView.count(
    // 한 행에 표시할 자식의 개수 지정
    crossAxisCount: 2,
    // 자식 위젯들 사이의 수직 간격 지정
    mainAxisSpacing: 16,
    // 자식 위젯들 사이의 좌우 간격 지정
    crossAxisSpacing: 15,
    // Gridview가 필요한 공간만 차지하도록 하는 옵션
    shrinkWrap: true,
    // Gridview 영역에서 스크롤을 금지하기 위한 옵션
    physics: const NeverScrollableScrollPhysics(),
    // padding값 지정
    padding: const EdgeInsets.all(0),
    // 자식 위젯의 가로 세로 비율 지정
    childAspectRatio: 1 / 1,
    // 자식 위젯 지정
    children: const [
    ],
),
```
위와 같은 방법으로 사용할 수 있다.  
Gridview는 따로 영역에 대한 설정을 해주지 않으면 높이가 무한이 되어서 오류가 발생할 수 있다.  
이때 shrinkWrap 옵션을 true로 설정해주면, Gridview가 무한히 영역을 확장하지 않고, Grid를 그리는데 필요한 공간만큼만 차지하게 된다.  
또는, shrinkWrap 옵션을 지정하지않고, Gridview를 Expanded 위젯으로 감싸주는 방법도 있다. 이러면 Gridview가 부모(Column, Row 등)의 영역을 꽉 채우게 된다.

그리고, Gridview는 자체적으로 스크롤을 기능을 지원한다.  
이 스크롤 기능은 딱 Gridview의 영역만 스크롤을 지원한다.  
따라서, 예를들어, 더 상위에서 SingleChildScrollView 등의 위젯을 통해 화면 전체를 스크롤하는 기능을 이미 적용했다면 스크롤 기능이 중복되어 적용될 수 있다.  
이때 'physics: const NeverScrollableScrollPhysics()' 를 적용해줌으로써 Gridview 내의 스크롤 기능을 off해 줄 수 있다.
