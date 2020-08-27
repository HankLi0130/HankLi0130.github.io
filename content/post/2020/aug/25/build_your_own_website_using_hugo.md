---
title: "Build your own website using Hugo"
date: 2020-08-25T17:25:39+08:00
draft: false
---

![Hogo Logo](/image/2020/aug/25/hugo_logo.png)

# Why Hugo?

As a developer, I need a website to recode the things I've done. I found out Hugo is perfect for blogs and it's easy to use. I'm gonna share how to create your own website using Hugo and deploy it to GitHub.

# Let's get it started

### 1. Install Hugo

I recommend [Homebrew](https://brew.sh/) to install Hugo because it's really easy if you're using macOS.

Open your terminal and type:
```
> brew install hugo
```

### 2. Create new website

```
> hugo new site [name]
```

The above will create a new Hugo site in [name] folder
 
### 3. Download theme you like

Go to [Hugo themes](https://themes.gohugo.io/), pick one you like and copy the address (using https instead of ssh of theme repository)

```
> cd [name]
> git init
> git submodule add [theme repository] themes/[theme name]
```
### 4. Set Configuration

Follow instruction of the theme. Usually in the page of the theme.

### 5. Wirte a new post

```
> hugo new posts/[post name]
```

It creates a new post in content folder. You have to write it with markdown syntax.

### 6. Test your website

```
> hugo server -D
```

Open a new tab in your brower and type "localhost:1313" and you'll see your website.

### 7. Deploy to GitHub page

```text
> git submodule add -b master https://github.com/<USERNAME>/<USERNAME>.github.io.git public

// create deploy.sh and chmode +x

> ./deploy.sh [commit message]
```

### 8. Enjoy your website

Type "[username].github.it" and the website will show up.