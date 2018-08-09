---
layout: post
title: Activity Indicator의 사용방법
cover: cover.jpg
date: 2018-08-09 11:20:00
categories: posts
---

최근 delegate의 사용방법을 익히고 있다 그 중 미니 브라우저를 만들면서<br/>
다른사이트로 이동시 Activity Indicator를 사용한 로딩서클을 보여주는 방법에 대해 기록 해보려 한다.<br/>


1. 먼저 스토리보드에 webkit view와 Activity Indicator view를 넣는다.
2. delegate처리를 할 수 있게 WKNavigationDelegate 프로토콜을 추가
3. 뷰컨트롤러에서 컨트롤 할 수 있도록 @IBOulet을 추가한다.

```swift
class ViewController: UIViewController, WKNavigationDelegate{

  @IBOutlet weak var loadingCircle: UIActivityIndicatorView!
  @IBOutlet weak var mainWebView: WKWebView!
}
```

여기서 다룰 부분은 webkit view의 시작과 끝을 알아야만 언제 시작되고 종료될지 알수 있기 때문에 그부분을 알 수 있는 함수를 사용하여 애니메이션을 시작, 종료하는 방법만 다룬다.

```swift
//didStartProvisionalNavigation - 시작 부분
func webView(_ webView: WKWebView, didStartProvisionalNavigation navigation: WKNavigation!) {
        loadingCircle.startAnimating()
}

//didFinish - 종료 부분
func webView(_ webView: WKWebView, didFinish navigation: WKNavigation!) {
        loadingCircle.stopAnimating()
}
```

이렇게만 하면 돌지 않는다. delegate부분을 연결해주어야한다.
일반적이라면 메인스토리보드에서 드래그앤드롭으로 delegate연결이 가능하지만
webkit view에서는 해당 기능을 지원하지 않는다.
때문에 소스코드상에서 delegate를 연결해주어야한다.

방법은 아래와 같다

viewdidload부분에 연결하는 코드한줄 적어 넣으면 끝
```swift
override func viewDidLoad() {
      super.viewDidLoad()
      mainWebView.navigationDelegate = self
}
```

그리고 이렇게 하더라도 Activity Indicator는 사라지지 않고 계속 있는데
이럴때는 선택 후 attribute inspector에 가서
<font color='red'>'Hides When Stopped'</font>
옵션을 체크해주면 시작과 끝에서만 보여지게 된다!
