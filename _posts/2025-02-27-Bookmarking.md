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
  box-shadow: 0 4px 6px rgba(0,0,0,0.05);
  transition: all 0.3s ease;
}

.bookmark-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 15px rgba(99,102,241,0.1);
}

/* 卡片内容样式 */
.card-header {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1rem;
}
.card-name{
  color: rgb(114, 223, 186);
  font-weight: 600;
}

.favicon {
  max-width: 40px;
  max-height: 40px;
  border-radius: 6px;
}

.card-link {
  color: #3b82f6;
  text-decoration: none;
  display: block;
  border: 1px solid red;
}

.card-link:hover {
  text-decoration: underline;
}

.tag {
  display: inline-block;
  background: rgba(99,102,241,0.1);
  color: #6366f1;
  padding: 0.3rem 0.8rem;
  border-radius: 999px;
  font-size: 0.9em;
  margin: 0.2rem;
}

@media (prefers-color-scheme: dark) {
  .bookmark-card {
    background: #2d3748;
    color: white;
  }
}
</style>

<div class="bookmark-container">
  {% for category in site.data.Bookmarking.categories %}
  <section class="category-section">
    <h2 class="category-title">
      <span class="category-icon">{{ category.icon }}</span> {{ category.name }}
    </h2>

    <div class="card-grid">
      {% for item in category.items %}
      <div class="bookmark-card">
        <a class="card-link" href="{{ item.url }}" target="_blank" rel="noopener noreferrer">
          <div class="card-header">
            <h3 class="card-name">{{ item.name }}</h3>
            <img class="favicon" src="{{ item.icon }}" alt="{{ item.name }}"/>
          </div>
          <p class="card-desc">{{ item.description }}</p>
          <div class="tags">
			{% for tag in item.tags %}
			<span class="tag">{{ tag }}</span>
			{% endfor %}
          </div>
        </a>
      </div>
      {% endfor %}
    </div>
  </section>
  {% endfor %}
</div>


