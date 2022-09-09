# Model-View-ViewModel Pattern  
## Model-View-ViewModel 패턴이란?  
* `Model-View-ViewModel` (MVVM) 은 오브젝트들을 세 개의 다른 그룹으로 나누는 구조적인 디자인 패턴입니다.  
  * Model: `App data` 가지고 있습니다. 대부분 struct나 class 입니다.  
  * View: visual element 를 보여주고 screen 을 컨트롤합니다. 대부분 UIView 입니다.  
  * View Models: model 을 View에 띄울 수 있는 value 로 바꾸는 역할을 합니다. 대부분 class 지만, reference 로 넘겨집니다.  
* `Model-View-Controller` (MVC) 패턴과 비슷하게 들립니다.  
* 상단의 다이어그램을 보면, view controller 가 있긴 하지만, role 이 축소되었습니다.  

## 언제 MVVM 패턴을 사용해야 하나요?  
* model 들을 view 에서 다르게 표현해야 할 때 이 패턴을 사용하세요.  
* 예를 들어서 view model 에서 `Date` 형식의 데이터를 date-formatted `String` 으로, `Decimal` 을 currency-formatted `String` 으로 변환할 수 있습니다.  
* MVC 패턴을 생각해보면, view controller 에서 너무 많은 일을 합니다.  
* `viewDidLoad` 도 핸들링해야하고, 다른 view lifecycle event 들도 핸들링해야 하며, `IBActions` 을 통해서 view callback 도 핸들링해야 합니다.  
* 그래서 종종 MVC 패턴을 `MVC: Massive View Controller` 라고도 부릅니다.  

## MVVM 패턴을 사용할 때 주의해야 할 점  
* `model-to-view` transformation 을 많이 사용해야 한다면 유용합니다.  
* 하지만 모든 element 들이 model, view, view model 에 들어맞지 않을 수도 있습니다.  
* 이럴 경우에는 MVVM 패턴과 더불어 다른 디자인 패턴을 활용할 수 있어야 합니다.  
* 또한, 처음 어플리케이션을 생성할 때는 MVVM 패턴이 그리 유용하지 않을 수도 있습니다.  
* MVC 로 시작하는 것이 더 나을 수도 있습니다.  
* Requirement 에 따라 다른 디자인 패턴을 골라서 적용하는 것이 좋습니다.  
* 변화를 두려워하지 마십시오 - 대신 계획을 세워 변화시키세요.  

## Key Points  
* MVVM 패턴은 view controller 들의 역할을 줄여주는 것을 도와줍니다.  
* "Massive View Controller" problem 을 해결해줍니다.  
* View model 에서는 object 를 가지고 변환시킵니다. 변환된 object 들은 view controller 에 전달되고, view 에 띄워집니다.  
* 특히 `Date` 나 `Decimal` 과 같은 computed property 들을 `String` 같이 `UILabel` 과 `UIView` 에 띄울 수 있도록 변환시키는데 유용합니다.  
* 하나에 view 에서만 view model 을 사용한다면 모든 configuration 을 view model 에 넣는것이 유용하겠지만, 여러개의 view 를 사용한다면 그렇지 않습니다.  
* 만약 앱의 크기가 작다면 `MVC` 로 시작하는 것이 유용합니다. 나중에 requirement 에 따라 다른 디자인 패턴을 사용하는 것이 좋습니다.  

## References  
* Design Patterns by Tutorials (raywenderlich)  
