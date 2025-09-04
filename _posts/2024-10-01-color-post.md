---
layout: post
title: MyBomberMan
tags: [Test, Color]
color: brown
/*author: sylhare*/
categories: Example
feature-img: "assets/img/feature-img/bom.png"
thumbnail: "assets/img/thumbnails/feature-img/bom.png"
excerpt_separator: <!--more-->
---

<div style="display: flex; gap: 20px; flex-wrap: wrap; margin-bottom: 20px;">
  <div style="flex: 1; min-width: 250px; border: 1px solid #444; border-radius: 8px; padding: 15px; background-color: #1c1c1c; color: #fff;">
    <h3>About</h3>
    <p>
      <strong>Wothingthing</strong>은 팀 프로젝트로 진행된 2D 횡스크롤 게임입니다.<br>
      본 프로젝트는 <strong>Alpha Engine</strong>이라는 <strong>2D렌더링 툴</strong>을 활용하여 게임을 구현했습니다.<br>
      이 경험을 통해 처음으로 팀과 협업하며 게임을 만들어보았습니다. 
	  <br>이 경험을 통해 팀과 협업할 시 서로의 의견을 조율해가는 능력을 기룰 수 있었고
	  또한,게임 내 2D 캐릭터 애니메이션 처리 과정, 충돌 처리, 컴포넌트 시스템 기능을 구현하며 문제 해결 능력을 기를 수 있었습니다.
	  <br>      
    </p>
  </div>

  <div style="flex: 1; min-width: 250px; border: 1px solid #444; border-radius: 8px; padding: 15px; background-color: #1c1c1c; color: #fff;">
    <h3>Project Info</h3>
    <ul>
      <li><strong>Role:</strong> AI & Gameplay Programmer / Team Leader</li>
      <li><strong>Team Size:</strong> 4명 (프로그래머 3명)</li>
      <li><strong>Time Frame:</strong> 3개월</li>
      <li><strong>Engine:</strong> Unreal Engine (Blueprint)</li>
    </ul>
  </div>
</div>

---


# What a colorful post!

This is an idea that came from [xukimseven/HardCandy-Jekyll](https://github.com/xukimseven/HardCandy-Jekyll) 
looking at this cheerful and colorful theme, I wanted to enable something similar for Type-on-Strap.

You can go fork and star _HardCandy-Jekyll_ too! 😉

<!--more-->

## How does it work?

Basically you need to add just one thing, the color:

```yml
---
layout: post
title: Color Post
color: brown
---
```

It can either be a html color like `brown` (which look like red to me). Or with the rgb:

```yml
---
layout: post
title: Color Post
color: rgb(165,42,42)
---
```

The background used is `lineart.png` from [xukimseven](https://github.com/xukimseven) you can edit it in the config file. 
If you want another one, put it in `/assets/img` as well. 

> ⚠️ It's a bit hacking the css in the `post.html`
