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
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
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
  grid-template-areas:
	  "icon name"
	  "desc desc";
  grid-template-columns: auto 1fr;
  gap: 1.5rem 2rem;
}

.card-header {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1rem;
}
.card-name{
  grid-area: name;
  color: rgb(114, 223, 186);
  font-weight: 600;
  font-size: 1.5rem;
  line-height: 1.3;
  align-self: center;
}

.div-img{
  grid-area: icon;
  width: 100px;
  height: 100px;
  aspect-ratio: 1/1;
  border-radius: 14px;
  padding: 12px;
  display: grid;
  justify-items: center;
  align-items: center;
}
.favicon {
  width: 100%;
  height: auto;
  object-fit: contain;
  transition: transform 0.3s ease;
}

.card-link {
  color: #3b82f6;
  text-decoration: none;
  display: block;
}

.card-link:hover {
  text-decoration: underline;
}
.card-link:hover .favicon {
  transform: scale(1.08);
}

.card-desc{
  grid-area: desc;
  color: var(--text-secondary, #666);
  font-size: 1rem;
  line-height: 1.7;
  margin: 0;
}

@media (prefers-color-scheme: dark) {
  .bookmark-card {
    background: #2d3748;
    color: white;
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
        <a class="card-link" href="{{ item.url }}" target="_blank" rel="noopener noreferrer">
          <div class="card-content">
            <div class="card-name">{{ item.name }}</div>
            <div class="div-img"><img class="favicon" src="{{ item.icon }}" alt="{{ item.name }}"/></div>
            <p class="card-desc">{{ item.description }}</p>
          </div>
        </a>
      </div>
      {% endfor %}
    </div>
</div>
{% endfor %}


