# Human Interface Guideline - iOS  
## 목차  
* [Themes](#themes)  
* [Interface Essentials](#interface-essentials)  
* [References](#references)

## Themes  
### iOS Design Themes  

* iOS가 다른 플랫폼들과 차별화되는 부분을 세가지로 나눌 수 있습니다.  

**Clarity**  
* 시스템 전반적으로, 모든 사이즈의 글씨가 읽기 편하고 아이콘이 명확하고 이해하기 쉽습니다.  
* 장식들은 섬세하고 적절하며, 기능에 초점을 맞추어 디자인 되어 있습니다.  
* 여백, 컬러, 폰트, 그래픽, 인터페이스 요소들은 중요한 내용을 강조하도록 맞춰져 있고,  상호작용이 잘 이뤄집니다.  

**Deference**
* 부드러운 모션과 아름다운 인터페이스는 사람들이 (앱을) 더 잘 이해할 수 있도록 도와줍니다.  
* 컨텐츠는 보통 전체 화면을 사용하며, (그 화면 중) 반투명과 흐린 화면들이 더욱 도움을 줍니다.  
* 베젤, 그라디언트, 그림자를 최소로 사용해서 인터페이스를 밝고 가볍게 유지합니다.  

**Depth**
* 선명한 시각적 레이어들과 현실감 있는 모션들은 (앱의) 이해를 돕고, 생기를 불어넣으며 체계를 잡아줍니다.  
* 터치와 discoverability는 기능을 더 잘 사용할 수 있게 해주며, 맥락을 잃지 않은 채로 추가적인 컨텐츠에 도달할 수 있도록 합니다.  
* 화면 전환은 컨텐츠를 여행하는 느낌을 제공합니다.  

### Design Principles  
> (앱의) 영향력과 접근성을 극대화시키기 위해서, 이러한 원칙들을 지키는 것이 좋습니다.  

**Aesthetic Integrity**  
* Aesthetic Integrity는 앱의 외형과 액션이 기능과 얼마나 연관되어 있는지를 나타냅니다.  
* 중요한 일을 해야 하는 앱의 경우에는, 사소하고 눈에 띄지 않는 그래픽, 스탠다드 컨트롤, 예측가능한 행동들을 사용해서 사람들을 계속 집중할 수 있게 만듭니다.  
* 하지만 게임과 같이 몰입형 앱의 경우에는 (사용자들이 직접) 발견할 수 있도록 장려하면서, 즐거움을 보장하는 매혹적인 외형을 제공합니다.  

**Consistency**  
* 일관성 있는 앱은 시스템이 제공하는 인터페이스, 잘 알려진 아이콘, 표준 텍스트 스타일, 한결같은 용어를 사용해서 친숙한 스탠다드와 패러다임을 구현합니다.  

**Direct Manipulation**  
* 컨텐츠를 직관적으로 조작할 수 있게 만들면 사람들이 더 쉽게 이해할 수 있습니다.  
* 사용자들은 컨텐츠를 조작하기 위해서 기기를 돌리거나 제스쳐를 이용할 때, 직관적 조작을 경험할 수 있습니다.  
* 직관적 조작을 통해서 사용자들은 그들의 행동에서의 즉각적이고 시각적인 결과를 볼 수 있습니다.  

**Feedback**  
* 피드백은 (사용자가) 행동하고 있음을 알려주고, 사람들이 인지하고 있을 수 있도록 결과를 보여줍니다.  
* Built-in iOS 앱들은 사용자의 모든 행동에 대해 반응해서, (사용자들이) 인지할 수 있는 피드백을 제공합니다.  
* 상호작용을 하는 요소들은 탭했을 때 잠시동안 하이라이트되며, 진행 상황을 알려주는 요소들은 길게 진행되는 작업의 상태를 실시간으로 보여줍니다.  
* 애니메이션과 소리는 (사용자의) 행동의 결과를 이해하기 쉽게 도와줍니다.  

**Metaphors**
* 사람들은 앱의 가상 물체와 행동들이 (현 세계 혹은 디지털 세계에서 겪은) 친숙한 경험들과 비슷하면 더 빠르게 배울 수 있습니다.  
* 사람들이 물리적으로 화면과 상호작용하기 때문에, 메타포어(비유)는 iOS에서 더 효과를 발휘합니다.  
* 사용자들은 아래에 있는 컨텐츠를 보기 위해서 뷰를 밀어서 보내고, 컨텐츠를 끌고 당깁니다.  
* 사용자들은 스위치를 껐다켰다 하고, 슬라이더를 움직이고, 픽커 벨류를 통해서 스크롤합니다.  
* 책과 잡지의 경우 다음장으로 넘기는 행동을 합니다.  

**User Control**  
* iOS를 통해서 사람들을 컨트롤 할 수 있는 상태에 둘 수 있습니다.  
* 앱은 action의 course를 제안할 수 있고, 위험한 결과에 대해 경고할 수 있습니다.  
* 하지만 앱이 decision-making까지 맡아서 진행하면 안됩니다.  
* 최고의 앱들은 '사용자들이 마음대로 조작하게 하는 것'과 '원하지 않는 결과를 피하는 것 사이'에서 밸런스를 잘 맞출 수 있습니다.  
* 상호작용하는 요소들을 친숙하고 예측가능하게 만들게 되면, 앱은 사람들이 앱을 통제한다고 느낄 수 있게 만들 수 있습니다.  
* 이 과정에서 사용자들은 해가 되는 행동을 확인할 수 있고, 심지어 그들이 진행 중인 상황에서도 쉽게 작업을 취소할 수 있습니다.  

## Interface Essentials  
* 대부분의 iOS 앱들은 UIKit의 component를 사용하여 빌드됩니다.  
* UIKit은 공통 interface element들을 정의하고 있는 programming framework 입니다.  
* UIKit은 앱이 시스템 전반적으로 일관성 있는 외관을 갖게 하며, 동시에 높은 수준의 customization도 제공합니다.  
* UIKit은 유연하고 친숙하며, 어떤 iOS 기기에도 잘 어울리는 app을 만들 수 있게 해줍니다.  
* System에서 appearance change가 있어도 자동으로 업데이트됩니다.  
* UIKit의 interface element들은 세 가지로 나눌 수 있습니다.  

### Bars  
* 앱에서 사용자들이 어디에 위치해 있는지 말해줍니다.  
* navigation을 제공합니다.  
* Action을 시작하거나 information을 주고받을 수 있는 button이나 elements를 포함하고 있습니다.  

### Views  
* 사용자들이 앱을 사용할 때 처음으로 보게 되는 text, graphics, animations, interactive elements와 같은 content를 담고 있습니다.  
* Scrolling, insertion, deletion, arrangement를 가능하게 해줍니다.  

### Controls  
* Action을 시작하고, 정보를 전달해줍니다.  
* Button, switch, text field, progress indicator 등이 있습니다.  

### 추가 설명  
* 기능적 측면에서도 UIkit을 이용하면, 앱이 사용자의 gesture에 반응할 수 있으며, drawing, accessibility, printing과 같은 기능을 할 수 있게 됩니다.  
* iOS는 Apple Pay, HealthKit, ResearchKit과 같은 다른 프로그래밍 프레임워크와 테크놀로지와도 긴밀하게 통합(연계)되어 있습니다.  

## References  
* https://developer.apple.com/design/human-interface-guidelines/ios/overview/themes/
* https://developer.apple.com/design/human-interface-guidelines/ios/overview/interface-essentials/
