# gonzalo-chumillas.github.io
Personal Website.

## Install

Download the repository and run the [Jekyll serve](https://jekyllrb.com/docs/usage/) to test it:
```bash
# replace MY_SITE by your folder name
git clone https://github.com/gchumillas/less_is_more MY_SITE
cd MY_SITE
bundle exec jekyll serve
```

## Front Matter configuration

The `menu_ordeer` attribute makes your page appear in the 'top menu' in a specific order. If it is missing, the page is not displayed in the 'top menu'.

```yaml
---
title: Page Title
permalink: /contact/

# Layout can be either 'default', 'home' or 'post'
layout: default

# This optional attribute makes your page appear in the top menu in a specific order
menu_order: 6
---
```

And that's all! Enjoy it!
