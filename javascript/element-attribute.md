---
title: "[JS] Element, Attribute이란?"
date: "2022-12-20"
category: "javascript"
tags: ["javascript", "elements", "attribute"]
---

# [JS] Element, Attribute이란?

```html
<div class="poster">
  <figure>
    <img src="img/poster08.jpg" srcset="img/poster08@2.jpg 2x" alt="침묵" />
  </figure>
  <div class="rank"><strong>4</strong></div>
  <div class="mx">
    <span class="icon m ir_pm">MX</span>
    <span class="icon b ir_pm">Boutique</span>
  </div>
</div>
```

<br>

## Element

<b>Element</b>은 \<div>\<figure>\<img>\<span>\<string> 등 각각의 태그들을 가리킨다.

<br>

## Attribute

<b>Attribute</b>은 Element 즉 각각의 태그들이 가지고 있는 class, src, srcset, alt, href 등 이런 것들을 가리킨다.

또한 Attribute는 element의 형식을 지정한다.

<br>

## Object

<b>Object(개체/객체)</b>는 우리가 구분할 수 있는 것들을 말하고 JavaScript와 DHTML에서 사용하는 최상위이다.

창을 가리키는 window, body를 가리키는 document, link, form, layer, all 등 페이지에 있는 모든 것들은 하나의 object이다.

이 object는 계층이 있고 각자에 지정된 역할을 수행한다.

프로그램 제작 시 윈도우 폼에 들어가는 커맨드 버튼이라든가 텍스트 박스 등 프로그램 작성 중에 사용되는 컨트롤들을 Object(개체)라고 부른다.

Object에 지정되어 있는 것은 Property, Method, Event가 있다

<br>

## Property

<b>Property(속성)</b>는 Object(개체)가 가지는 성질(구성요소)을 나타낸다. 개체에서 보이는 이름, 색, 위치값 등 개체가 갖는 성질이다. 이러한 개체의 성질은 사용자가 접근하여 수정이 가능하다. 예를 들어 window object가 가지고 있는 property로는 name, parent, history, innerHeight, self 등등 object가 이들의 property를 가지고 object 자신의 역할을 한다.

<br>

## Method

<b>Method</b>는 Object(개체)가 어떤 동작을 할 수 있는 일이다.

각 Object(개체)에 지정된 Method로 각각의 Object(개체)를 사용할 수 있다.

window의 method는 open(), alert(), close(), scroll(), setTimeout() 등이 있고 다른 예로 텍스트 박스의 내용을 지운다던가, 포커스를 하는 것들이 있다..

- 개체의 속성과 메소드를 구분하는 방법은 따로 없다. 많이 하다 보면 개체의 속성인지 메서드인지 알 수 있다. 속성은 어떠한 값을 가져야 하기 때문에 뒤에 '='이 붙고 우변에 어떤 값이 온다. 하지만 메서드는 어떠한 일을 하고 그 일을 수행한 후에 결과를 반환할 수도 있고 그렇지 않을 수도 있다. 그래서 어떠한 값을 갖는 것이 아니기 때문에 메서드 뒤에는 '='가 붙지 않는다.

<br>

## Event(이벤트)

<b>Event(이벤트)</b>는 개체와는 별개라고 본다. Event(이벤트)는 윈도우 상에서 발생하는 사건들을 말한다. 각 object에는 그 일어난 동작을 수용할 수 있는 event 종류가 지정되어 있는데 예를 들면 윈도 창에서 마우스를 클릭한다던가, 이동하거나 드래그, 스크롤링, 키보드 버튼 누르기, 창 닫기 등 윈도 창에서 발생할 수 있는 여러 가지 사건들을 뜻한다.

링크(\<a>)의 event로는 mouseover, mouseout, click, mousedown, mouseup, dblclick 등이 있다.

<br>

## Event Handler

<b>Event Handler</b>는 event를 다룰 때 사용하는 형식의 이름이다. 링크에서 click이벤트를 다루고자 한다면 \<a href="xxx.html" onClick="btnClicked">처름 onClick으로 click 이벤트를 지정하는 것이다.

<br>

## ⚡참조

- <https://enterkey.tistory.com/164>
