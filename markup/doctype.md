---
title: "[HTML] Doctype이란?"
date: "2022-12-25"
category: "markup"
tags: ["html", "doctype"]
---

# [HTML] Doctype이란?

## DOCTYPE이란?

Doctype(Doccument Type Definition, DTD)이란 웹 페이지들의 html 코드를 확인해보면 맨 위에 \<!DOCTYPE html>이라고 선언되어 있는 부분이 있는데 이 것은 DTD라고 한다.

위키백과에 따르면 DTD는 컴퓨터 용어로 SGML(Standard Generalized Markup Language 문서용 마크업 언어를 정의하기 위한 메타 언어) 계열의 마크업 언어에서 문서 형식을 정의하는 것이다. SCML을 비롯해 HTML, XHTML, XML 등에서 쓰인다. 즉, 문서 형식에 대한 정의를 하고 수많은 Tag와 버전들 사이에서 어떠한 규격의 내용을 사용할 것인지를 정의하는 것이다. 브라우저에게 '이 문서가 어떤 문서 형식을 따르고 있다'라고 알려주는 것이다..

<br />

## DOCTYPE을 사용하는 이유

각 브라우저의 엔진이 렌더링 모드를 결정할 때 문서 첫 부분의 DOCTYPE을 참조하는데 이 것을 선언하지 않으면 브라우저가 해당 문서의 형식을 알 수 없게 된다. 브라우저는 '표준 모드 (Standard Mode)'가 아닌 '비표준 모드(Quirks Mode)로 렌더링 모드를 변경하게 되면서 문서 제작자가 의도한 레이아웃(태그 구조, css 등)이 깨지게 되어 사용자에게 정상적인 상태의 문서를 보여주지 못하게 된다.

다시 말해, DTD를 어떻게 선언하느냐에 따라 브라우저의 렌더링 모드가 바뀌게 되고 사용할 수 있는 태그와 속성이 바뀌게 된다. DTD를 선언하지 않을 경우 브라우저가 표준모드가 아닌 비표준모드(Quirks Mode)로 렌더링되어 크로스 브라우징에 어려움을 겪게 된다. 크롬에서는 비표준일지라도 잘 보여주지만 IE8과 같은 경우는 레이아웃이 깨져 보일 수 있다. 크로스 브라우징의 가장 기본적인 시작은 표준 모드 준수이다!!

<br />

## DOCTYPE 종류별 선언

// DTD 선언은 태그가 아니기 때문에 닫는 부분은 없고 웹 표준에 있어 가장 먼저 선언되어야 한다!!!

```html
HTML 4.01 Strict
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

HTML 4.01 Transitional
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

HTML 4.01 Frameset
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">

XHTML 1.0 Strict
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

XHTML 1.0 Transitional
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

XHTML 1.0 Frameset
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">

XHTML 1.1
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

HTML5
<!DOCTYPE html>
```
