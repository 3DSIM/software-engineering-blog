# software-engineering-blog
This is the source for 3dsim.github.io.  We use Hugo (https://gohugo.io) as our static site generator.  

See this blog post if you want to see how we set this up: http://3dsim.github.io/using-hugo-and-wercker-to-create-and-automate-your-own-site/

## Contributing a blog post

We follow the git flow process, so you'll need to fork the main repo and contribute PRs.  To create a post, once you've forked and cloned the repo...

* Create a new branch.  `git checkout -b <branch name>`
* If you haven't already, add your author info to the `config.toml`.  

e.g.

```
[author.ryanwalls]
name = "Ryan Walls"
bio = "Software Engineering Manager at 3DSIM"
location = "Park City, UT"
website = "https://twitter.com/ryanwalls"
thumbnail = "https://s.gravatar.com/avatar/c9079ca00a3b670571d25f2e23023d0e?s=80"
```

* Create a new post.  

`hugo new post/Some-Title-with-Proper-Casing.md`

* Edit the post.  Use front matter similar to the following:

```
+++
author = "ryanwalls"
comments = true
date = "2016-05-28T15:51:23-06:00"
draft = false
title = "Your Amazing Title"
+++
```

* Preview your changes: `hugo server --buildDrafts=true`

* Commit your code and submit a PR.  
