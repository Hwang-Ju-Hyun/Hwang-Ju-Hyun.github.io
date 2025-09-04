---
layout: page
title: github
subtitle: From the pexels folder
permalink: /gallery/
gallery_path: "assets/img/pexels"
excluded: true
position: 3
tags: [Page]
---

This is a photo gallery made from the static files in the `assets/img/pexels` folder. 
I wanted to automatically create a simple gallery from a folder without having to create a markdown page as you would for the portfolio.


{% include gallery.html gallery_path=page.gallery_path %}

<!-- GitHub 버튼 추가 -->
<div style="margin-top: 20px;">
  <a href="https://github.com/Hwang-Ju-Hyun" target="_blank" style="text-decoration: none;">
    <button style="padding: 10px 20px; font-size: 16px; background-color: #24292e; color: white; border: none; border-radius: 5px; cursor: pointer;">
      내 깃허브 방문
    </button>
  </a>
</div>
