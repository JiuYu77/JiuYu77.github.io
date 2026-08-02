---
title: 网址收藏
description: 网址推荐：操作系统、实用软件、社区、动漫...。
author: yu
date: 2025-02-27 00:00:00 +0800
categories: [Blogging, Demo]
tags: [website]
pin: true
---

<style>
/* 自定义卡片容器 */
.bookmark-container {
  max-width: 1200px;
  margin: 2rem auto;
  padding: 0 20px;
}

/* 分类标题样式 */
.category-title {
  font-size: 1.8rem;
  color: #2d3748;
  border-left: 4px solid #6366f1;
  padding-left: 1rem;
  margin: 3rem 0 1.5rem;
  background: linear-gradient(90deg, rgba(99,102,241,0.1) 0%, transparent 100%);
}

/* 卡片布局 */
.card-grid {
  display: grid;
  gap: 1.5rem;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
}

/* 单张卡片样式 */
.bookmark-card {
  position: relative;
  background: white;
  border-radius: 12px;
  padding: 1rem;
  box-shadow:
      0 2px 8px rgba(0,0,0,0.05),
      0 12px 24px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
  border: 1px solid rgba(0, 0, 0, 0.05);
}

.bookmark-card:hover {
  transform: translateY(-6px);
  box-shadow:
      0 8px 32px rgba(99,102,241,0.15),
      0 16px 48px rgba(99,102,241,0.1);
}

/* 卡片内容样式 */
.card-content{
  display: grid;
  grid-template-areas: "icon name";
  grid-template-columns: auto 1fr;
  gap: 0 2rem;
  align-items: center;
}

.card-name{
  grid-area: name;
  color: rgb(114, 223, 186);
  font-weight: 600;
  font-size: 1.2rem;
  line-height: 1.3;

  height: 80px;
  display: flex;
  align-items: center;
}

.div-img{
  position: relative;
  grid-area: icon;
  width: 80px;
  height: 80px;
  border-radius: 14px;
  display: grid;
  justify-items: center;
  align-items: center;
  overflow: hidden;
  border: 1px solid rgba(0, 0, 0, 0.05);
}
/* 构建后主题会把 img 包成 a（class 迁移到 a 上），用绝对定位强制水平垂直居中 */
.div-img a{
  position: absolute;
  inset: 0;
  display: block;
}
.div-img a img{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  max-width: 100%;
  max-height: 100%;
  width: auto;
  height: auto;
}
.favicon {
  width: 100%;
  height: auto;
  transition: transform 0.3s ease;
}

.card-link {
  text-decoration: none;
  display: block;
}

.card-link:hover {
  text-decoration: underline;
}
.card-link:hover .favicon {
  transform: scale(1.08);
}

/* 鼠标悬停时显示的网站简介 */
.card-tooltip {
  position: absolute;
  bottom: calc(100% + 14px);
  left: 50%;
  transform: translateX(-50%) translateY(8px);
  background: rgba(17, 24, 39, 0.95);
  color: #fff;
  font-size: 0.875rem;
  line-height: 1.7;
  padding: 0.75rem 1rem;
  border-radius: 10px;
  width: max-content;
  max-width: 300px;
  text-align: center;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.25);
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.25s ease, transform 0.25s ease;
  z-index: 100;
  pointer-events: none;
  word-break: break-word;
}

.card-tooltip::after {
  content: "";
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border: 7px solid transparent;
  border-top-color: rgba(17, 24, 39, 0.95);
}

.bookmark-card:hover .card-tooltip {
  opacity: 1;
  visibility: visible;
  transform: translateX(-50%) translateY(0);
}

@media (prefers-color-scheme: dark) {
  .bookmark-card {
    background: #2d3748;
    color: white;
  }

  .category-title {
    color: #cccccc;
  }
}
</style>

{% for category in site.data.Bookmarking.categories %}
<div class="bookmark-container">
    <h2 class="category-title" id="{{ category.name }}">
      <span class="category-icon">{{ category.icon }}</span> {{ category.name }}
      <a href="#{{ category.name }}" class="anchor text-muted"><i class="fas fa-hashtag"></i></a>
    </h2>

    <div class="card-grid">
      {% for item in category.items %}
      <div class="bookmark-card">
          <div class="card-content">
            <div class="div-img"><img class="favicon" src="{{ item.icon }}" alt="{{ item.name }}"/></div>
            <a class="card-link img-link" href="{{ item.url }}" target="_blank" rel="noopener noreferrer">
              <div class="card-name">{{ item.name }}</div>
            </a>
          </div>
        <div class="card-tooltip">{{ item.description }}</div>
      </div>
      {% endfor %}
    </div>
</div>
{% endfor %}


