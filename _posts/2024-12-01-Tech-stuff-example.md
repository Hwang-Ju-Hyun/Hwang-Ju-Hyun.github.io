---
layout: post
title: Gonjiamerica
color: rgb(250, 50, 50)
feature-img: "assets/img/feature-img/g.jpeg"
thumbnail: "assets/img/thumbnails/feature-img/g.jpeg"
tags: [Highlight, Markdown]
---

<div style="display: flex; gap: 20px; flex-wrap: wrap; margin-bottom: 20px;">
  <div style="flex: 1; min-width: 250px; border: 1px solid #444; border-radius: 8px; padding: 15px; background-color: #1c1c1c; color: #fff;">
    <h3>About</h3>
    <p>
      <strong>Gonjiamerica</strong>는 팀 프로젝트로 진행된 1인칭 공포 FPS 게임입니다.<br>
      본 프로젝트를 통해 <strong>언리얼 엔진</strong>을 처음 접했고, 빠른 프로토타이핑과 학습을 위해
      <strong>블루프린트</strong>를 활용하여 게임을 구현했습니다.<br>
      이를 통해 게임 엔진의 기초 개념부터 AI 시스템, 애니메이션 및 내비게이션 시스템까지 
      실제로 경험하고 적용할 수 있었습니다.
    </p>
  </div>

  <div style="flex: 1; min-width: 250px; border: 1px solid #444; border-radius: 8px; padding: 15px; background-color: #1c1c1c; color: #fff;">
    <h3>Project Info</h3>
    <ul>
      <li><strong>Role:</strong> AI & Gameplay Programmer</li>
      <li><strong>Team Size:</strong> 4명 (프로그래머 3명)</li>
      <li><strong>Time Frame:</strong> 3개월</li>
      <li><strong>Engine:</strong> Unreal Engine (Blueprint)</li>
    </ul>
  </div>
</div>

<iframe width="960" height="515" src="https://www.youtube.com/embed/gzlIk9uziLk" 
        title="Gonjiamerica Demo" frameborder="0" 
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
        allowfullscreen></iframe>

---

## Introduction
이 프로젝트는 제가 언리얼 엔진을 처음 다뤄본 경험이자, 게임 엔진의 다양한 기능을 체험할 수 있었던 소중한 기회였습니다.  
특히 블루프린트를 중심으로 개발하며 빠른 프로토타이핑이 가능했고, 게임 엔진의 기초 메커니즘을 익히는 데 큰 도움이 되었습니다.  

이를 통해 단순히 기능 구현에 그치지 않고, **AI 설계, 내비게이션 시스템, 애니메이션 전환**까지 직접 경험하며 게임 프로그래밍의 주요 영역을 폭넓게 학습할 수 있었습니다.  

---

## My Contributions
- **AI 시스템 구현**  
  - **Behavior Tree**를 활용하여 몬스터의 탐색, 추적, 공격 등의 행동 로직을 설계 및 구현  
  - 다양한 상황에서 유연하게 반응할 수 있도록 태스크와 데코레이터를 구조화  

- **네비게이션 시스템 (Nav Mesh)**  
  - 몬스터가 장애물을 회피하며 플레이어를 추적할 수 있도록 Nav Mesh 적용  
  - 동적으로 이동 가능한 경로를 학습하고 테스트하면서 내비게이션의 원리를 이해  

- **애니메이션 상태 머신 (Animation State Machine)**  
  - 몬스터의 이동, 공격, 피격 등 상황별 애니메이션 전환을 구현  
  - 자연스러운 상태 전환과 게임플레이 몰입도를 높이는 애니메이션 제어 경험  

- **환경 효과**  
  - 안개, 공기 흐름 등 분위기 연출 요소를 추가하여 공포감을 강화  

---

## Why Behavior Tree?

AI를 설계할 때 처음에는 단순한 **Finite State Machine(FSM)** 구조를 고려했습니다.  
FSM은 구현이 간단하고 직관적이며, 적은 수의 상태에서는 빠르게 동작한다는 장점이 있습니다.  
하지만 몬스터의 **탐색 → 추적 → 공격 → 피격 → 재탐색** 등 복잡한 행동 패턴을 점차 추가하면서 몇 가지 한계가 드러났습니다:

- **상태 폭발 문제**: 상태와 전환 조건이 늘어날수록 코드 복잡도가 기하급수적으로 증가  
- **재사용성 부족**: 특정 행동 로직이 여러 상태에서 필요할 경우, 중복 코드가 발생  
- **확장성 제약**: 새로운 행동을 추가하려면 기존 상태 전환 로직을 전부 수정해야 하는 비효율성  

이러한 이유로, 저는 **Behavior Tree(BT)** 기반의 구조를 선택했습니다.  
BT는 FSM에 비해 다음과 같은 강점을 가집니다:

- **모듈화된 구조**: 태스크(Task) 단위로 행동을 정의할 수 있어, 중복 없이 재사용 가능  
- **유연한 확장성**: 새로운 행동을 추가할 때 기존 트리를 크게 수정할 필요가 없음  
- **조건 기반 제어**: 데코레이터(Decorator)와 서비스(Service)를 통해 상황별 의사결정 가능  
- **가독성 향상**: 트리 형태로 시각화되어, 협업 시 AI 로직을 쉽게 이해하고 디버깅 가능  

결과적으로, **Behavior Tree를 사용함으로써 FSM의 복잡도를 줄이고 유지보수성·확장성을 확보**할 수 있었으며, 실제로 프로젝트의 몬스터 AI가 보다 자연스럽고 유연하게 플레이어에게 반응하도록 구현할 수 있었습니다.

## Search on code

Search should be working even for complicated escape symbols.

```bash
sed -i 's/\"hostname\"\:.*$/\"hostname\"\: \"'$IPADDR'\"\,/g' open-falcon/agent/config/cfg.json
```

Or try searching for partial of a command, like this article should be returned when looking for "find grep"

```bash
find /etc -type f -exec cat '{}' \; | tr -c '.[:digit:]' '\n' | grep '^[^.][^.]*\.[^.][^.]*\.[^.][^.]*\.[^.][^.]*$'
```

## Code highlighting examples

Because you might put code in your blog post, and you want to make sure it will look good in here. Plus that the search
function will still be working!

### XML

Example from [W3C]
```xml
<part number="1976">
  <name>Windscreen Wiper</name>
  <description>The Windscreen wiper
    automatically removes rain
    from your windscreen, if it
    should happen to splash there.
    It has a rubber <ref part="1977">blade</ref>
    which can be ordered separately
    if you need to replace it.
  </description>
</part>
```

### Java

java example

```java
import java.util.*;

@Example
public class Demo {
  private static final String CONSTANT = "String";
  private Object o;
  /**
   * Creates a new demo.
   * @param o The object to demonstrate.
   */
  public Demo(Object o) {
    this.o = o !== null ? o : new Object();
    String s = CONSTANT + "Other example of text";
    int i = 123 - 33 % 11;
  }
  public static void main(String[] args) {
    Demo demo = new Demo();
    System.out.println(demo.o.toString())
  }
}
```

### Javascript

```javascript
/**
 * Does a thing
 */
function helloWorld(param1, param2) {
    const example = `hello ${param1}`
    var something = {
        key: "value",
        number: 1
    };

    // Do something
    if (2.0 % 2 == something) {
        console.log('Hello, world!');
    } else {
        return null;
    }

    // TODO comment
}
```

### JSON

```json
{
  "animals": {
    "tiger": {
      "name": "tiger",
      "images": ["🐯", "🐅", "⻁"]
    },
    "turtle": {
      "age": 126,
      "image": "🐢"
    },
    "unicorn": {
      "doesExist": true,
      "image": "🦄"
    }
  }
}
```

### Python

```python
import os


def some_function(param_one="", param_two=0):
    r'''A docstring'''
    if param_one > param_two:  # interesting
        print("Greater")
    return (param_two - param_one + 1 + 0b10) or None


class SomeClass:
    """ dunno what I am doing """

    def __init__(self):
        pass
```

### YAML

You can also render some yaml, like this `_config.yml`:

```yml

# Welcome to Jekyll!
#
# This config file is meant for settings that affect your whole blog, values
# which you are expected to set up once and rarely edit after that. If you find
# yourself editing this file very often, consider using Jekyll's data files
# feature for the data you need to update frequently.
#
# This file, "_config.yml" is *NOT* reloaded automatically when you use
# 'bundle exec jekyll serve'. If you change this file, please restart the server process.

# Site settings
# These are used to personalize your new site. If you look in the HTML files,
# you will see them accessed via {{ site.title }}, {{ site.email }}, and so on.
# You can create any custom variable you would like, and they will be accessible
# in the templates via {{ site.myvariable }}.

# SITE CONFIGURATION
baseurl: "/Type-on-Strap"
url: "https://sylhare.github.io"

# THEME-SPECIFIC CONFIGURATION
title: Type on Strap                                    # site's title
description: "A website with blog posts and pages"      # used by search engines
avatar: assets/img/triangle.png                         # Empty for no avatar in navbar
favicon: assets/favicon.ico                             # Icon displayed in the tab

remote_theme: sylhare/Type-on-Strap                     # If using as a remote_theme in github
```

[W3C]: https://www.w3.org/standards/xml/core
