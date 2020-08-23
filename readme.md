# DevMigration

DevMigration is a Blog designed to help others using [**Hugo**](https://gohugo.io/) to read interesting articles and comments on topics posted on the Blog. There are differents types/categories of posts:

 - Links
 - Articles
 - Videos

If you would like to fork this repository and create your own just follow the links in the installation section.

### Installation
To used this repository you first need to install [**Hugo**](https://gohugo.io/) following the instructions on their website.

When you create an application using Hugo you can install themes that help you give life to your application. If you like this theme you can install the original [**Theme**](https://github.com/Lednerb/bilberry-hugo-theme) and following the installation steps.

### Sections
As you noticed from the theme instructions you can use MD(Markdown) to create your site, what I will do is explain a bit what are some of the properties that you will see here and that I use to create an article or a link and so on.

``` md
---
title: "Best title ever"
date: 2020-08-17T23:02:54-04:00
draft: false

categories: ['Links']
tags: ['Hugo','Themes', 'Markdown']

# Set your external url
link: "https://www.some-website.com"
---
Por Renso Diaz.
```
When you created a link type of post as you can see from the **MD** above, you have multiple properties that are on top of the body of your post wrap in six hyphen " --- ---", This are some of them but you can go into more depth with Hugo into how these are made:

 - **Title**: Name on your post.
 - **Date**: When was your article created.
 - **Draft**: when working with Hugo you can tell if this article is done to be shown or not by using this property, If this property is set to true this article won't be shown when you upload your content.
 - **Categories**: Every post on this theme uses a category tag to help you organize all the post that you are going to be uploading.
 - **Tags**: Tags will help you give the reader an idea of what are some of the important words that you are using in your post.
 - **Link**: This property helps us to let the theme know what type of post we want to display and structure itself as it should.
 - **Body**: Here you can add your paragraphs, images, tables. Etc.