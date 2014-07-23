---
layout: post
title:  "Blog Moved to GitHub Pages"
date:   2014-07-16 00:00:00
comments: true
categories: []
---

Moved this much neglected blog from [wordpress.com](http://wordpress.com/) to [GitHub Pages](https://pages.github.com/). This is an overview of the steps used to migrate the blog.

### Step 1: Install Jekyll

`$ gem install jekyll`

then get your development system up and running:

`$ jekyll new jonathanoneill.github.io`

`$ cd jonathanoneill.github.io`

this creates the file structure:

_config.yml<br>
_layouts<br>
_posts<br>
_site<br>
css<br>
index.html

start server:

`$ jekyll serve —watch`

your default web server is now running at http://0.0.0.0:4000

### Step 2: Import content from wordpress.com

There are a number of importers to allow you to [import](http://import.jekyllrb.com) your existing blog for use with Jekyll. In this case my existing blog was on wordpress.com so I used its [importer](http://import.jekyllrb.com/docs/wordpressdotcom/).

Export blog content from wordpress.com:

*   http://jonathanoneill.wordpress.com/wp-admin/export.php?type=export
*   Select “All content"
*   Click “Download Export File”, this downloads to an xml file e.g. wordpress.xml

`$ gem install jekyll-import`

`$ ruby -rubygems -e 'require "jekyll-import";
    JekyllImport::Importers::WordpressDotCom.run({
      "source" => "wordpress.xml"
    })`

this command will import your wordpress posts into the _posts directory.

### Step 3: Format Pages

A few further things are required:

*   Customize the css and page templates
*   Move images from wordpress.com to the assets/blog directory and update any links in the posts

### Step 4: Push to GitHub

Create a public repository called:

https://github.com/jonathanoneill/jonathanoneill.github.io

commit and push Jekyll files to the repository. The site will then be automaticaly available at [http://jonathanoneill.github.io](http://jonathanoneill.github.io) (it may take a few minutes).

### Step 5: Update DNS

Update DNS record to customize the domain name.