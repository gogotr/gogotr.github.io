---
layout: post
title: SWIFT XML Parsing
cover: cover.jpg
date: 2018-08-28 11:20:00
categories: posts
---
##SWIFT XML Parsing
- - -

오늘은 간단한 xml파싱 방법을 숙지하였다.

다른 언어에서도 마찬가지로 xml파싱은 어렵지 않으나

delegate를 사용하는 점에서 차이를 보였다.

아직 delegate 사용에 익숙치 않은 나에게 다시금 delegate의 사용및 개념을 확고히 하고자 xml파싱을 살펴보았다.

- - -


간단하게 테이블뷰를 생성하고 날씨정보를 출력 해보았다

1. XMLParserDelegate를 추가한다.
2. XMLParser 객체를 생성하고
3. delegate 함수 3개를 추가한다


```swift
//
//  ViewController.swift
//  StarDust
//
//  Created by 이상묵 on 2018. 8. 27..
//  Copyright © 2018년 이상묵. All rights reserved.
//

import UIKit

class ViewController: UIViewController, UITableViewDataSource, XMLParserDelegate {

    var dataList = [[String:String]]()
    var detailData = [String:String]()

    override func viewDidLoad() {
        super.viewDidLoad()
        print("hello")
        // Do any additional setup after loading the view, typically from a nib.
        let urlString = "주소"

        guard let baseURL = URL(string: urlString) else {
            print("URL error")
            return
        }

        guard let parser = XMLParser(contentsOf: baseURL) else
        {
            print("parse error : can't read Data")
            return
        }

        parser.delegate = self
        if !parser.parse(){
            print("parse fail")
        }

    }

    func parser(_ parser: XMLParser, didStartElement elementName: String, namespaceURI: String?, qualifiedName qName: String?, attributes attributeDict: [String : String] = [:]) {
        print(elementName)
    }

    func parser(_ parser: XMLParser, foundCharacters string: String) {
        print(string)
    }

    func parser(_ parser: XMLParser, didEndElement elementName: String, namespaceURI: String?, qualifiedName qName: String?) {
        print(elementName)
    }

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return dataList.count
    }

    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    	//커스텀셀 생성
        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath) as! weatherCell

        var dicTemp = dataList[indexPath.row]
        let weatherStr = dicTemp["weather"]
        let countryNm = dicTemp["country"]
        let tempStr = dicTemp["tempature"]

        cell.lblName.text = countryNm
        cell.lblTemp.text = tempStr
        cell.lblWeather.text = weatherStr

        return cell
    }
}
```
이제는 익숙해진 테이블 뷰, 역시 여러번 반복 학습을 하면 이해가 된다.

오늘의 핵심 아래 3가지를 기억하자
###didStartElement : xml시작요소를 알 수 있다. ###
###foundCharacters : 현재요소의 값을 알 수 있다. ###
###didEndElement : xml마지막요소를 알 수 있다. ###


```SWIFT
func parser(_ parser: XMLParser, didStartElement elementName: String, namespaceURI: String?, qualifiedName qName: String?, attributes attributeDict: [String : String] = [:]) {
        print(elementName)
}

func parser(_ parser: XMLParser, foundCharacters string: String) {
        print(string)
}

func parser(_ parser: XMLParser, didEndElement elementName: String, namespaceURI: String?, qualifiedName qName: String?) {
        print(elementName)
}
```
