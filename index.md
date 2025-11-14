---
layout: default
title: Home
---

<style>
  /* General style */
  .main-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
  }

  /* Table of contents */
  .toc {
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid #333;
    border-radius: 8px;
    padding: 20px;
    margin-bottom: 40px;
  }
  
  .toc-title {
    font-size: 1.3em;
    font-weight: bold;
    margin-bottom: 15px;
    color: #fff;
  }
  
  .toc-section {
    margin: 8px 0;
    padding-left: 10px;
  }
  
  .toc-section a {
    text-decoration: none;
    color: #8ab4f8;
  }
  
  .toc-section a:hover {
    text-decoration: underline;
  }
  
  .toc-items {
    margin-left: 25px;
    margin-top: 5px;
    font-size: 0.9em;
  }
  
  .toc-item {
    margin: 3px 0;
    color: #aaa;
  }

  /* Sections */
  .section {
    margin: 50px 0;
    padding: 30px;
    background: rgba(255, 255, 255, 0.02);
    border-radius: 10px;
    border-left: 4px solid #4A90E2;
  }
  
  .section-header {
    border-bottom: 2px solid #333;
    padding-bottom: 15px;
    margin-bottom: 25px;
  }
  
  .section h1 {
    margin: 0;
    font-size: 2em;
    color: #fff;
  }
  
  .section-count {
    font-size: 0.8em;
    color: #888;
    margin-top: 5px;
  }

  /* Items */
  .item {
    margin: 25px 0;
    padding: 20px;
    background: rgba(0, 0, 0, 0.3);
    border-radius: 6px;
    border: 1px solid rgba(255, 255, 255, 0.1);
  }
  
  .item-header {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: 15px;
  }
  
  .item h2 {
    margin: 0;
    font-size: 1.4em;
    color: #8ab4f8;
  }
  
  .item-meta {
    display: flex;
    gap: 15px;
    font-size: 0.85em;
    color: #888;
    margin-bottom: 15px;
  }
  
  .item-content {
    line-height: 1.6;
    color: #ddd;
  }
  
  .item-content h3 {
    color: #fff;
    margin-top: 20px;
  }
  
  .item-content code {
    background: rgba(255, 255, 255, 0.1);
    padding: 2px 6px;
    border-radius: 3px;
    font-family: 'Courier New', monospace;
  }
  
  .item-content pre {
    background: #1e1e1e;
    padding: 15px;
    border-radius: 5px;
    overflow-x: auto;
  }

  /* Status badges */
  .status-badge {
    padding: 3px 10px;
    border-radius: 12px;
    font-size: 0.8em;
    font-weight: bold;
    text-transform: uppercase;
  }
  
  .status-stable {
    background: #28a745;
    color: white;
  }
  
  .status-review {
    background: #ffc107;
    color: #000;
  }
  
  .status-draft {
    background: #6c757d;
    color: white;
  }

  /* Edit button */
  .edit-btn {
    font-size: 0.85em;
    color: #8ab4f8;
    text-decoration: none;
    padding: 5px 10px;
    border: 1px solid #8ab4f8;
    border-radius: 4px;
  }
  
  .edit-btn:hover {
    background: rgba(138, 180, 248, 0.1);
  }

  /* Empty section */
  .empty-section {
    padding: 20px;
    text-align: center;
    color: #666;
    font-style: italic;
  }

  /* Footer stats */
  .stats-footer {
    margin-top: 60px;
    padding: 30px;
    border-top: 1px solid #333;
    text-align: center;
    color: #888;
  }
  
  .stats-footer a {
    color: #8ab4f8;
    text-decoration: none;
  }
</style>

<div class="main-container">

# {{ site.title }}

{{ site.description }}

<!-- Automatic table of contents -->
<div class="toc">
  <div class="toc-title">📑 Table of Contents</div>
  {% for section_name in site.section_order %}
    {% assign section_meta = site.section_metadata[section_name] %}
    {% assign items = site[section_name] | sort: 'order' %}
    
    <div class="toc-section">
      <a href="#{{ section_name }}">
        {{ section_meta.title }}
        {% if items.size > 0 %}
          <span style="color: #666;">({{ items.size }})</span>
        {% endif %}
      </a>
      
      {% if items.size > 0 and items.size <= 5 %}
        <div class="toc-items">
          {% for item in items %}
            <div class="toc-item">
              ↳ <a href="#{{ section_name }}-{{ item.title | slugify }}" style="color: #aaa;">
                {{ item.title }}
              </a>
            </div>
          {% endfor %}
        </div>
      {% elsif items.size > 5 %}
        <div class="toc-items">
          <div class="toc-item" style="color: #666;">
            ↳ {{ items.size }} items
          </div>
        </div>
      {% endif %}
    </div>
  {% endfor %}
</div>

<!-- Compile all sections -->
{% for section_name in site.section_order %}
  {% assign section_meta = site.section_metadata[section_name] %}
  {% assign items = site[section_name] | sort: 'order' %}
  
  <section id="{{ section_name }}" class="section">
    <div class="section-header">
      <h1>{{ section_meta.title }}</h1>
      <div class="section-count">
        {% if items.size == 0 %}
          No items yet
        {% elsif items.size == 1 %}
          1 item
        {% else %}
          {{ items.size }} items
        {% endif %}
      </div>
    </div>
    
    {% if items.size > 0 %}
      <!-- Display all items in this section -->
      {% for item in items %}
        <article id="{{ section_name }}-{{ item.title | slugify }}" class="item">
          <div class="item-header">
            <h2>{{ item.title }}</h2>
            {% if item.status %}
              <span class="status-badge status-{{ item.status }}">
                {{ item.status }}
              </span>
            {% endif %}
          </div>
          
          <!-- Item metadata -->
          <div class="item-meta">
            {% if item.author %}
              <span>👤 {{ item.author }}</span>
            {% endif %}
            {% if item.date %}
              <span>📅 {{ item.date | date: site.minima.date_format }}</span>
            {% endif %}
            {% if item.tags %}
              <span>🏷️ {{ item.tags | join: ", " }}</span>
            {% endif %}
          </div>
          
          <!-- Item content -->
          <div class="item-content">
            {{ item.content }}
          </div>
          
          <!-- Edit button -->
          <div style="margin-top: 20px; text-align: right;">
            <a href="https://github.com/aifortimeseries/ai4ts.github.io/edit/main/{{ item.path }}" 
               class="edit-btn" 
               target="_blank">
              ✏️ Edit on GitHub
            </a>
          </div>
        </article>
      {% endfor %}
      
    {% else %}
      <!-- Empty section -->
      <div class="empty-section">
        <p>This section is currently empty. Be the first to contribute!</p>
        <p style="margin-top: 15px;">
          <a href="https://github.com/aifortimeseries/ai4ts.github.io/new/main/{{ section_name }}" 
             class="edit-btn" 
             target="_blank">
            ➕ Add first item
          </a>
        </p>
      </div>
    {% endif %}
  </section>
{% endfor %}

<!-- Footer with statistics -->
<div class="stats-footer">
  <p>
    <strong>Repository Statistics:</strong><br>
    {% assign total_items = 0 %}
    {% for section_name in site.section_order %}
      {% assign items = site[section_name] %}
      {% assign total_items = total_items | plus: items.size %}
    {% endfor %}
    
    📊 Total items: {{ total_items }} • 
    📁 Sections: {{ site.section_order.size }} • 
    🔄 Last build: {{ site.time | date: "%Y-%m-%d %H:%M" }} UTC
  </p>
  
  <p>
    <a href="https://github.com/aifortimeseries/ai4ts.github.io" target="_blank">
      View on GitHub
    </a> • 
    <a href="https://github.com/aifortimeseries/ai4ts.github.io/issues/new" target="_blank">
      Report Issue
    </a> • 
    <a href="/feed.xml">
      RSS Feed
    </a>
  </p>
  
  <p style="margin-top: 20px; font-size: 0.9em;">
    Maintained by IEEE P3579 and W3C WAITS CG
  </p>
</div>

</div>
