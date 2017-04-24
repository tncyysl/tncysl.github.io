---
layout: post
title:  "deploy php app with heroku"
date:   Sun Mar 26 17:13:40 +03 2017
author: Tuncay UYSAL
categories: jekyll update
---

`brew tap homebrew/services`

`brew install homebrew/php/php71 --with-postgresql` **=>** `brew options php71

`brew install composer`

`brew install heroku`

```
heroku login
git clone https://github.com/heroku/php-getting-started.git
cd php-getting-started.git
heroku create
git push heroku master
heroku open
heroku logs --tail
```


https://devcenter.heroku.com/articles/getting-started-with-php#define-a-procfile
