# 쥬스 자판기 프로그램 토이 프로젝트

> **쥬스 자판기 프로그램** <br>
>
> 프로젝트 소개: SeSAC 토이 프로젝트 - 쥬스 자판기 프로그램   
> 프로젝트 기간: 2023.12.04 ~ 2023.12.22

## 팀원 소개
| <img src="https://github.com/tasty-code/ios-rock-scissor-paper/assets/63277563/129dca38-b8b6-401e-893d-5a6ba5e0f927.png" width="200"> | <img src="https://github.com/jane1choi/ios-juice-maker/assets/63277563/7fbc1d13-79fa-41ab-9bd4-452da98b0094.png" width="200"> |
|:----------:|:---------:|
| **Jane** | **Harry** |
| [@jane1choi](https://github.com/jane1choi) |  [@hemil0102](https://github.com/hemil0102)) | 
<br>

## 구현 요구 사항
### Step 1 : 쥬스 메이커 타입 정의
- `FruitStore.swift` 파일에 과일의 재고를 관리하는 FruitStore 타입을 정의합니다.
- FruitStore는 다음의 조건을 충족해야 합니다.
    - FruitStore가 관리하는 과일의 종류 : 딸기, 바나나, 파인애플, 키위, 망고
    - 각 과일의 초기 재고 : 10개
    - 각 과일의 수량 n개를 변경하는 기능이 있습니다.
- JuiceMaker.swift 파일에 다음의 조건을 충족하는 JuiceMaker 타입을 정의합니다.
    - FruitStore의 과일을 사용해 과일쥬스를 제조합니다.
        - 딸기쥬스 : 딸기 16개 소모
        - 바나나쥬스 : 바나나 2개 소모
        - 키위쥬스 : 키위 3개 소모
        - 파인애플 쥬스 : 파인애플 2개 소모
        - 딸바쥬스 : 딸기 10개 + 바나나 1개 소모
        - 망고 쥬스 : 망고 3개 소모
        - 망고키위 쥬스 : 망고 2개 + 키위 1개 소모
    - 과일의 재고가 부족하면 과일쥬스를 제조할 수 없습니다.

### Step 2 : 초기화면 기능구현
- 화면 전환 구현
  - 쥬스 재료의 재고가 있는 경우 : 쥬스 제조 후 `“*** 쥬스 나왔습니다! 맛있게 드세요!”` 얼럿 표시
  - 쥬스 재료의 재고가 없는 경우 : “`재료가 모자라요. 재고를 수정할까요?”` 얼럿 표시
    - `‘예'` 선택시 재고수정 화면으로 이동
    - `‘아니오'` 선택시 얼럿 닫기
과일쥬스를 제조하여 과일의 재고가 변경되면 화면의 적절한 요소에 변경사항을 반영합니다.

### Step 3 : 재고 수정 기능구현
- 화면 제목 `'재고 추가`' 및 `'닫기'` 버튼 구현
- 닫기를 터치하면 이전화면으로 복귀
- 화면 진입시 과일의 현재 재고 수량 표시
- `UIStepper`를 통한 재고 수정
- iPhone 15 외에 다른 시뮬레이터에서도 UI가 정상적으로 보일 수 있도록 오토레이아웃 적용
 
### ⚠️ 제약사항
- 코드에 느낌표(!)를 사용하지 않습니다.
- Swift API Design Guidelines의 문서대로 코드를 작성합니다.
- 코드에 주석을 남기지 않습니다.
- 외부 라이브러리를 사용하지 않습니다.

## 폴더 구조
```
🧃JuiceMaker
├── 📁Constant
│   ├── Juice.swift
│   ├── Fruit.swift
│   └── JuiceMakerError.swift
├── 📁Controller
│   ├── OderViewController.swift
│   ├── InventoryViewController.swift
│   ├── AppDelegate.swift
│   └── SceneDelegate.swift
├── 📁Model
│   ├── JuiceMaker.swift
│   └── FruitStore.swift
├── 📁View
│   ├── Assets.xcassets
│   ├── LaunchScreen.swift
│   └── Main.storyboard
├── 📁Extension
│   └── UIViewController + Extension.swift.swift
└── Info.plist
```
## 클래스 다이어그램
![Juicemaker_ClassDiagram_20231222 drawio](https://github.com/jane1choi/ios-juice-maker/assets/63277563/670a4314-355f-4ef6-be0b-16a051c5b1ff)

## 구현 사항
1. `Fruit` 열거형 - 과일 종류 정의
2. `Juice` 열거형 - 쥬스 종류 지정 및 각 쥬스의 과일 요구 수량 정의
3. `JuiceMakerError` 열거형 - 쥬스 제조 실패 에러 케이스 정의
4. `FruitStore` 클래스 - 과일 재고 데이터 정의 
    - `[Fruit: Int]` 타입 과일 재고 프로퍼티를 정의
    - 은닉화, 캡슐화로 재고 접근 및 수정
5. `JuiceMaker` 구조체 - 쥬스 제조 및 재고 부족시 에러 전달
    - 재고가 충분하다면 보유한 재고에서 요구 수량을 빼주며 재고를 업데이트한다.
    - 보유한 재고와 요구 수량을 비교하여 재고 부족시 에러를 `throw`한다.
    - 두 가지 재료를 갖는 쥬스는 `for문`으로 재고 충분 여부를 확인한다.
        - 재료 중 하나라도 부족하다면 쥬스 제조 실패하며 재고를 수정하지 않는다.
6. 에러 처리 
    - `do`, `try`, `catch`문을 활용하여 에러를 처리한다.
    - 쥬스 제조 성공 또는 실패 시 적절한 Alert을 표시한다.  
7. `UIAlertController`를 이용한 Alert 구현 
    - `presentAlert()`을 통해 쥬스 제조 성공시 확인 버튼을 구현
    - `presentAlertWithCancel()`을 통해 쥬스 재고 부족시 재고 수정 화면 이동 및 취소 구현
8. 화면 전환 구현
    - 모달 방식의 화면 전환을 구현
    - `instantiateViewController(identifier, creater)` 메서드의 `creator`를 통해서 `InventoryViewController`에 과일 재고를 전달하며 의존성을 주입
    - `presentingViewController`를 활용하여 `dismiss`시 커스텀 메서드를 통해 이전 뷰(`OrderViewController`)로 전달
## 구동 화면
![Simulator Screen Recording - iPhone 14 Pro - 2023-12-19 at 21 07 32](https://github.com/tasty-code/ios-juice-maker/assets/63277563/2090ad0f-5dc1-4682-befe-350c592e4ac0)

