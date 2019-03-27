## 自定义wordpress主题

### 1. 整体布局分类
* header
* content
* sidebar
* footer

### 2. style.css
> 设置自定义主题整体布局样式，注释部分决定了wordpress能不能获取自定义主题
```
@charset 'UTF-8';
/*
Theme Name: 自定义主题标题，该注释决定了wordpress能否预读到主题相关信息
*/
```

### 3. index.php
> 显示网站首页，也可以理解成最终的模板文件。

> 如果没有访问到应有的资源，默认显示404.php，如果不存在404.php，就查找上一级search.php，一直按优先级显示。

> 优先级顺序：

> index.php <- single.php <- page.php <- archive.php <- search.php <- 404.php

```
index.php
<?php
// 获取头部模板，默认先从当前theme文件查找header.php，如果没有，就从wp_includes/theme-compat下获取通用header.php模板
get_header();
?>
<div id="page">
  <div id="header">
    <h1><a href="<?php bloginfo('url'); ?>" title="<?php bloginfo('name'); ?>"><?php bloginfo('name'); ?></a></h1>
    <p><?php bloginfo('description'); ?></p>
  </div>
  <div id="content">
    <?php /* php嵌入的注释不会显示在源代码 while start */ ?>
    <?php if (have_posts()): ?>
    <?php while (have_posts()): the_post(); ?>
    <div class="post" id="post-<?php the_ID() ?>">
      <!- 博文标题及链接 ->
      <h2><a href="<?php the_permalink(); ?>" rel="bookmark" title="<?php the_title(); ?>"><?php the_title(); ?></a></h2>
      <!- 发表日期 ->
      <div class="post-date">
        <span class="post-month"><?php the_time('M'); ?></span>
        <span class="post-day"><?php the_time('d'); ?></span>
      </div>
      <!- 作者 ->
      <span class="post-author"><?php _e('Author'); ?>: <?php the_author(',') ?></span>
      <!- 类别 ->
      <span class="post-category"><?php _e('Categories'); ?>: <?php the_category(','); ?></span>
      <!- 注释 ->
      <span class="post-comments"><?php comments_popup_link('No Comments &raquo;', '1 Comment &raquo;', '% Comments &raquo;'); ?></span>
      <!- 内容 ->
      <div class="entry">
        <?php the_content('更多内容 &raquo;'); ?>
      </div>
    </div>
    <?php endwhile; ?>
    <?php endif; ?>
  </div>
  <div id="sidebar"></div>
  <div id="footer"></div>
</div>


/**
 bloginfo('url')          返回网站主页地址
 bloginfo('name')         返回网站标题
 bloginfo('description')  返回网站描述
 have_posts()             判断是否还有文章
 the_post()               类似文章指针，移动指针到当前读取文章，之后的the_ID什么的都基于这一步
 */
```
