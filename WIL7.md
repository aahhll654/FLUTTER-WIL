# WIL7

## 패키지 사용
이번 과제에서는 geolocator 패키지와 google_maps_flutter 패키지를 사용해
출석체크 기능을 제공하는 앱을 구현하였다.  
패키지를 사용하려면 pubspec.yaml 파일 내 dependencies 부분에 사용하기를 원하는 패키지를 추가해주고
패키지를 install 한 후에, import 해야 사용할 수 있다.

## geolocator
geolocator 패키지는 현재 자신의 device의 위치를 찾을 때 사용하는 패키지이다.  
```dart
Geolocator.getCurrentPosition()
// 현재 자신의 위치의 위도, 경도를 찾는 함수

Geolocator.distanceBetween(
  double startLatitude,
  double startLongitude,
  double endLatitude,
  double endLongitude,
)
// 두 위치 사이의 distance를 구하는 함수
```

또한, 현재 device에서 위치 서비스가 활성화 되어있는지, 권한이 활성화 되어있는지 확인하고 요청하는 기능도 가지고 있다.
```dart
Geolocator.isLocationServiceEnabled()
// 위치 서비스가 활성화 되어있는지 확인하는 함수

Geolocator.checkPermission()
// 권한을 확인하는 함수

Geolocator.requestPermission()
// 권한 허가를 요청하는 함수
```

## google_maps_flutter
google_maps_flutter는 flutter 앱에 구글 지도를 추가해 줄 때 사용하는 패키지이다.  
먼저 이 패키지를 사용하기 위해서는 구글 클라우드 플랫폼에서 API 키를 발급받아야 한다. 키를 발급받으면 해당 키를 이용해서 앱에 구글 지도 위젯을 추가해 줄 수 있다.  
```dart
GoogleMap(
    initialCameraPosition: const CameraPosition(
        target: companyLatLng,
        zoom: 16,
    ),
    // 초기 시작시 지도의 어떤 부분을 나타낼지 결정하는 옵션
    // target에 LatLng 클래스를 입력
    // LatLng 클래스는 위도와 경도를 나타내는 클래스
    // zoom 옵션은 배율을 조정하는 옵션
    myLocationEnabled: true,
    // 내 위치를 나타낼지 결정하는 옵션, default 값은 false값임
    markers: {marker},
    // 지도에 marker를 나타내기 위한 옵션
    // 지도에 나타내고 싶은 marker들을 넣은 Set를 입력
    circles: {circle},
    // 특정 위치 주변에 원을 그리고 싶을 때 사용하는 옵션
    // maker와 마찬가지로 지도에 나타내고 싶은 circle들을 넣은 Set를 입력
),
```

## FutureBuilder
FutureBuilder란 Future를 사용할 때, 서버에서 데이터를 모두 받아오기 전에, 데이터 없이 그릴 수 있는 부분을 먼저 그려주기 위해 사용한다.  
예를 들어, 데이터를 받아오기전에는 화면에 로딩 이미지를 띄웠다가 데이터를 다 받아오면, 해당 데이터를 이용해 그리고 싶었던 화면을 다시 그려줄 수 있다.
```dart
FutureBuilder(
  future: checkPermission(),
  // future를 이용해 서버로 부터 데이터를 요청
  builder: (context, snapshot) {
    if (!snapshot.hasData &&
        snapshot.connectionState == ConnectionState.waiting) {
      return const Center(
        child: CircularProgressIndicator(),
        // 아직 데이터를 다 받아오지 못한 경우 로딩화면 표시
      );
    }

    return Center(
      child: Text(
        snapshot.data.toString(),
      ),
    );
    // 데이터를 다 받아온 경우 해당 데이터를 이용해서 화면을 구성
  }
),
```