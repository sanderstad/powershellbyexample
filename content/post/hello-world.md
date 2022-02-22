---
title: Hello World
date: '2022-02-22'
categories:
  - Example
---

```{powershell, comment='', echo=3, eval=Sys.which('bash') != '', message=FALSE}
cd ../..;
if [ ! -f 'theme.toml' ]; then exit 0; fi  # only run find below within the theme example site
find . -not -path '*/exampleSite/*' \( -name '*.html' -o -name '*.css' \) | xargs wc -l
```
