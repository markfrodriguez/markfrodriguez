+++
date = "2017-04-13T09:59:12-04:00"
description = ""
draft = false
slug = "hugo-setup"
tags = []
title = "Hugo Setup"
topics = []

+++

## Prerequisites

* Golang  installed
* GOPATH configured

## Install

```
go get -v github.com/spf13/hugo
```

Once the `get` completes, you should find the  `hugo` executable sitting inside `$GOPATH/bin/`.
To update Hugo’s dependencies, use `go get` with the `-u` option.

```
go get -u -v github.com/spf13/hugo
```

## Create Site

Navigate to a convenient location on your filesystem and create a new Hugo site `mysite` by executing the following command.

```
hugo new site mysite
```

## Structure Created

* **archetypes**: You can create new content files in Hugo using the `hugo new` command. When you run that command, it adds few configuration properties to the post like date and title. [Archetype](https://gohugo.io/content/archetypes/) allows you to define your own configuration properties that will be added to the post front matter whenever `hugo new` command is used.
* **config.toml**: Every website should have a configuration file at the root. By default, the configuration file uses `TOML` format but you can also use `YAML` or `JSON` formats as well. [TOML](https://github.com/toml-lang/toml) is minimal configuration file format that’s easy to read due to obvious semantics. The configuration settings mentioned in the `config.toml` are applied to the full site. These configuration settings include `baseURL` and `title` of the website.
* **content**: This is where you will store content of the website. Inside content, you will create sub-directories for different sections. Let’s suppose your website has three sections – `blog`, `article`, and `tutorial` then you will have three different directories for each of them inside the `content` directory. The name of the section i.e. `blog`, `article`, or `tutorial` will be used by Hugo to apply a specific layout applicable to that section.
* **data**: This directory is used to store configuration files that can be used by Hugo when generating your website. You can write these files in YAML, JSON, or TOML format.
* **layouts**: The content inside this directory is used to specify how your content will be converted into the static website.
* **static**: This directory is used to store all the static content that your website will need like images, CSS, JavaScript or other static content.
* **themes**: This is where you will create a theme for your site to use. Themes provide the layout and templates that renders content. There’s a wide variety of open-source themes available to download and use but you can also create your own if you prefer.

## Install a Theme

```
cd themes
git clone https://github.com/yoshiharuyamashita/blackburn.git
```

## Configure Site

Copy config.toml file from Blackburn template exampleSite to root of Hugo site.

```
baseurl = "http://www.willowbot.com/"
languageCode = "en-us"
title = "WillowBot Sandbox"
theme = "blackburn"
author = "WillowBot Team"
copyright = "&copy; 2017. All rights reserved."
canonifyurls = false
paginate = 10

[indexes]
  tag = "tags"
  topic = "topics"

[params]
  # Shown in the home page
  subtitle = "a more formal napkin"
  brand = "WillowBot"
  googleAnalytics = "UA-11111111-1"
  disqus = ""
  # CSS name for highlight.js
  highlightjs = "androidstudio"
  dateFormat = "Jan 02, 2006, 15:04"
  
[permalinks]
  post = "/:slug/"

[menu]
  # Shown in the side menu.
  [[menu.main]]
    name = "Home"
    pre = "<i class='fa fa-home fa-fw'></i>"
    weight = 0
    identifier = "home"
    url = "/"
  [[menu.main]]
    name = "Projects"
    pre = "<i class='fa fa-rocket fa-fw'></i>"
    weight = 1
    identifier = "projects"
    url = "/projects/"
  [[menu.main]]
    name = "Posts"
    pre = "<i class='fa fa-database fa-fw'></i>"
    weight = 1
    identifier = "post"
    url = "/post/"
  [[menu.main]]
    name = "Quotes"
    pre = "<i class='fa fa-quote-left fa-fw'></i>"
    weight = 1
    identifier = "quotes"
    url = "/quotes/"
  [[menu.main]]
    name = "About"
    pre = "<i class='fa fa-user fa-fw'></i>"
    weight = 2
    identifier = "about"
    url = "/about/"
```

Copy default.md file from blackburn/archetypes to archetypes directory in hugo root.

```
+++
title = ""
slug = ""
draft = true
tags = []
topics = []
description = ""

+++
```

## Creating Content

From the site root directory, execute

```
hugo new post/2017/0412-hello-world.md
```

This will create a new file ready for content. Set the front matter details as appropriate.

```
+++
date = "2017-04-12T14:03:36-04:00"
description = ""
draft = true
slug = "hello-world"
tags = []
title = "Hello World"
topics = []

+++
```

New posts default to drafts. To make a draft public, either run the command below or manually change the draft status in the post to `false`.

```
hugo undraft content/post/2017/0412-hello-world.md
```

Write up the article using [markdown](https://en.support.wordpress.com/markdown-quick-reference/)...

## Checking Content

Execute, the line below to start a local server that serves up the site.

```
hugo server --theme=blackburn --buildDrafts
```

To view the site, navigate to [http://localhost:1313/.](http://localhost:1313/) The hugo server supports live reloading so changes made to posts, config, etc. are automatically reflected on the served up web pages.

## Generate Web Site

To actually generate the static web pages for the site, run the command below.

```
hugo --theme=blackburn
```

After running the `hugo` command, a `/public` directory is created containing all the generated website source files ready for uploading “online”.  The site is hosted on [Google Cloud Storage](https://cloud.google.com/). Details about how that's done forthcoming.
