# RPLidar Windows Touch

Slamtec **RPLIDAR** 센서로 벽면을 터치 스크린처럼 만드는 Windows 애플리케이션.

벽 네 방향에 라이다를 설치해두면, 사람이 벽을 짚었을 때 그 좌표를 잡아 **Windows 마우스/터치 이벤트로 변환**합니다. 프로젝터로 벽에 화면을 쏘고 그 위를 직접 만지는 인터랙티브 설치물에 쓰였습니다.

## 동작 방식

```
RPLIDAR (1~4대)  →  거리/각도 스캔  →  화면 좌표 변환  →  mouse_event 주입  →  Windows
```

- 센서를 **1대에서 4대까지** 동시에 붙일 수 있고, 각 센서는 담당 모니터 인덱스를 가집니다
- 센서별 설치 위치·보정값은 설정 JSON에서 읽어옵니다 (`Constants.cs`의 `SENSOR_JSON_PATH`)
- 좌표 변환 후 `user32.dll`의 `mouse_event`를 P/Invoke로 호출해 실제 커서를 움직이고 클릭을 발생시킵니다

## 코드 구성

| 파일 | 역할 |
| --- | --- |
| `Scripts/RP_Script/RplidarBinding.cs` | Slamtec SDK 네이티브 DLL P/Invoke 바인딩 |
| `Scripts/RP_Script/RpSensorManager.cs` | 여러 센서의 생명주기·스레드 관리 |
| `Scripts/RP_Script/RpEachSensor.cs` | 센서 1대 단위의 스캔 처리 |
| `Scripts/RP_Script/RpLidarSensorData.cs` | 스캔 데이터(거리/각도) 구조 |
| `Scripts/RP_Script/RpMouseOperation.cs` | `mouse_event` 기반 커서 이동·클릭 주입 |
| `Scripts/Constants.cs` | 센서 설정 로드, 화면 크기 상수 |
| `Scripts/NTJsonDataClass.cs` | 설정 JSON 직렬화 클래스 |
| `Scripts/UnityQuaternion.cs` | Unity 쿼터니언 연산을 옮겨온 헬퍼 |

## 요구 사항

- .NET Framework 4.7.1 / 4.8
- Visual Studio (`RPLidar_WindowsTouch.sln`)
- **Slamtec RPLIDAR SDK 네이티브 DLL** (`dll/x86_64`) — 사용하는 센서 모델·SDK 버전에 맞는 것으로 교체해야 합니다

## 상태

현업에서 쓰이던 코드를 참고용으로 남겨둔 저장소입니다. 설정 경로가 당시 설치 환경 기준으로 하드코딩되어 있고 빌드 산출물(`bin/`, `obj/`)도 포함되어 있어, 그대로 실행하기보다 **라이다 → 터치 변환 구현 방식의 참고 자료**로 보시는 편이 맞습니다.
