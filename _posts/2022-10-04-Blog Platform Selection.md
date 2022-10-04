---
title: Blog Platform Selection
date: 2022-10-04 20:15:38 +0800
categories: [Technology, CMS]
tags: [jekyll, blogger, medium, wordpress, markdown]
---

My brother has recently adivsed me to write a technology blog to record my technology, I have reviewed several blogging platform and finally decide to use [Jekyll](https://jekyllrb.com/) after below comparisons. 

| Platforms  | Pros      | Cons         |
|-----------|-----------|--------------|
| Wordpress | - Tons of template<br>- Familiar to the framework<br>- SEO & plugins | - Backend hosting cost<br>- Visual editor generate problematic HTML code |
| Blogger   | - Easy to setup<br>- Good replacement of [Xanga](https://wikipedia.org/wiki/xanga)<br>- Easy setup for page view analytics or even adsense  | - Need to deal with HTML code if writing post with &lt;table&gt;<br> - Theme flexibility is very limited   |
| Medium    | - Support inserting code block<br> - Considerable monetization <br> - Great SEO optimization | - Poor support in using Chinese input method<br> - No custom domain unless paid  |
| Jekyll    | - Supporting [Markdown](https://en.wikipedia.org/wiki/Markdown)<br> - Lightweight and static website <br> - flexibility for configuration | - Technical barriers, e.g., [Ruby on Rails](https://rubyonrails.org/)<br> - Limited themes and plugins  |

Frankly speaking, starting Jekyll with [Apple M1](https://en.wikipedia.org/wiki/Apple_M1) is troublesome, it took me an hour to set up it with the basic template and several hours more for integrating with the theme. Nevertheless, it is fucking simple to write up a blog in Markdown such that every blog posts are fully compatible to any Jekyll theme. Even more, without dealing with the theme-specific HTML code or playing with fancy CSS, I can focus on writing the content and code.

```typescript
//Syntax highlight is super good
console.log("Hello World");
```