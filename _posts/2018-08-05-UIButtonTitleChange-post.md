---
layout: post
title: UIButton의 Title 변경과 UIControlState
cover: cover.jpg
date: 2018-08-05 01:55:00
categories: posts
---

오늘은 단순하지만 내부적인 변수의 정확한 의미와 사용을 위해 포스팅을 작성한다.

기존 버튼이나 레이블의 경우 @IBOulet 을 사용하여 접근 가능하도록 생성하고 해당 UI의 값이나 상태를 변경할 수 있다.

Button TItle의 변경은 간단하다.
```swift
@IBOutlet weak var startButton: UIButton!
startButton.setTitle(“Stop”, for: UIControlState.normal)
```

위와 같이 작성해준다면 UI버튼의 Title을 변경 할 수 있다.
그나저나 “Stop”이라는 매개변수는 알겠는데 그 옆에 UIControlState라는 매개변수가 보인다.

굉장히 거슬린다

애플 공식 문서로 확인 을 해보자

```
* Structure
- UIControlState
Constants describing the state of a control.
```

>  컨트롤의 상태를 설명하는 상수

라고 한다. 그럼 컨트롤들은 어떠한 상태를 가지고 있는가 살펴보자

![UIControlState]("/UIControlState.png")

위와 같이 총 7개의 상수가 존재한다  
각각의 의미를 알아보자


1. normal<br/>
 [static var normal: UIControlState](apple-reference-documentation://hsOohbJNGp)<br/>
The normal, or default state of a control—that is, enabled but neither selected nor highlighted.<br/>
**노말 상태, 즉 컨트롤의 디폴트 상태를 말한다**<br/>
사용상태지만 선택되지도 강조되지도 않는 상태를 말한다.<br/>
그냥 컨트롤 생성 했을 그대로의 상태를 말하는 것 같다.<br/>

2. highlighted<br/>
 [static var highlighted: UIControlState](apple-reference-documentation://hsR9R_AZcL)<br/>
 Highlighted state of a control.<br/>
강조된 컨트롤의 상태<br/>
**컨트롤을 선택하고 있을경우 이벤트가 발생한다..**<br/>
꾹 누르고 있으면 해당 이벤트들이 발생한다.<br/>

3. disabled<br/>
 [static var disabled: UIControlState](apple-reference-documentation://hsFhBCJA3W)<br/>
Disabled state of a control.<br/>
**말그대로 비활성화 시킨다.**

4. selected<br/>
 [static var selected: UIControlState](apple-reference-documentation://hsJ1xMyvqf)<br/>
Selected state of a control.<br/>
선택된 컨트롤 상태<br/>
**여러가지 컨트롤이 있을 때 선택된 상태를 알려줌**<br/>
테스트 해봣는데 title이 변경이 안된다...? 뭐지?<br/>

5. focused<br/>
 [static var focused: UIControlState](apple-reference-documentation://hsnHv89VWq)<br/>
Focused state of a control.<br/>
포커스된 컨트롤의 상태<br/>
**포커스를 수신하면 해당 상태로 값을 변경한다.**<br/>

6. application<br/>
 [static var application: UIControlState](apple-reference-documentation://hsCZCKhQ7E)<br/>
Additional control-state flags available for application use.<br/>
앱이 사용할 수 있는 추가적인 컨트롤의 상태값을 말한다.<br/>
잘 모르겠다...<br/>

7. reserved<br/>
 [static var reserved: UIControlState](apple-reference-documentation://hsKY4g9Wmw)<br/>
Control-state flags reserved for internal framework use.<br/>
내부 프레임워크에 사용되는 예약된 상태 값이라고 한다.<br/>
어떤 부분에 사용되는 것인가...<br/>


총 7개의 상태가 있는데 6, 7 번을 제외하고는 어떤상태인지 알 수 있었다.. 그러나 application, reserved에 대해서는 잘 모르겠다. 애플 공식 문서에도 저 문장 하나있고 끝이다...

어쨌거나

**버튼을 클릭하거나 어떠한 상태값이 변경될때 버튼의 title을 변경해줘야하는 경우는 반드시 생긴다. 이럴 때 위와 같은 상태 값을 잘 사용한다면 사용자에게 더욱 직관적이고 편리한 UI를 제공 할 수 있을 것이다.**
