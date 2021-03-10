---
title: "Build your own website using Hugo"
date: 2020-08-25T17:25:39+08:00
tags: [Hugo]
draft: false
---

![Hogo Logo](/posts/2020/0825/hugo_logo.png)

# Why Hugo?

As a developer, I need a website to recode the things I've done. I found out Hugo is perfect for blogs and it's easy to use. I'm gonna share how to create your own website using Hugo and deploy it to GitHub.

# Let's get it started

### Step 1: Install Hugo

I recommend [Homebrew](https://brew.sh/) to install Hugo because it's really easy if you're using macOS.

Open your terminal and type:
```text
> brew install hugo
```

### Step 2: Create new website

```text
> hugo new site [name]
```

The above will create a new Hugo site in `[name]` folder
 
### Step 3: Download theme you like

Go to [Hugo themes](https://themes.gohugo.io/), pick one you like and copy the address (using https instead of ssh of theme repository)

In my case, I'm using [Minimal](https://themes.gohugo.io/minimal/) for my blog.

```text
> cd [name]
> git init
> git submodule add [theme repository] themes/[theme name]
```

Go into the `[name]` folder and type git commands to apply a theme for your website.

### Step 4: Setting Configuration

Just follow instructions of the theme. Usually you can find it in the page of the theme.

You can find the Minial configuration [here](https://themes.gohugo.io/minimal/#configuration)

### Step 5: Let's wirte a new post

It's really easy to write a post using Hugo, just type the command below.

``` text
> hugo new posts/[post name].md
```

It creates a new post in `content` folder. You have to write it with markdown syntax.

### Step 6: Test your website

It's time to test the website, type the command below

```text
> hugo server -D
```

Open a new tab in your brower and type `localhost:1313`.

### Step 7: Deploy to GitHub page

First of all, you have to create 2 repositories on GitHub and then connect them to your project and website.

1. Create a `[YOUR-PROJECT]` (e.g. blog) repository on GitHub. This repository will contain Hugo’s content and other source files.

2. Create a `[USERNAME]`.github.io GitHub repository. This is the repository that will contain the fully rendered version of your Hugo website.

3. Add new remote in your existing Hugo project to the new local `[YOUR-PROJECT]` repository.

4. Type command below

```text
> git submodule add -b master https://github.com/<USERNAME>/<USERNAME>.github.io.git public
```

5. Make sure the baseURL in your config file is updated with: `[USERNAME]`.github.io

6. Create a [deploy.sh](https://gohugo.io/hosting-and-deployment/hosting-on-github/#put-it-into-a-script) and run it like

```text
> ./deploy.sh [commit message]
```

7. Type `[USERNAME].github.io` on your browser and you can see the website.

### Step 8: Enjoy your website
Since you've finished the steps above, you can create new posts easily and push them to the repository, it will update your website.

# References

- [Quick Start](https://gohugo.io/getting-started/quick-start/)

- [Host on GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/)