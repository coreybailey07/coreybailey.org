---
layout: post
title: Lazy Loading NVM
---

As with most developers experiencing significant slow down of shell
initialization post <code>nvm</code> installation, it became clear it
was a problem that would have to be solved. After searching the
web and referencing the <code>nvm</code> README for a solution, I came across an article
by [evanbrodie][00]. The solution was practical, but I
prefered a slightly dryer syntax which for my purposes replaced the multiple
function definitions with a <code>for</code> loop. 

```bash
export NVM_DIR=~/.nvm

lazy_load_nvm() {
  unset -f nvm node npm npx
  # Load nvm
  [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
  # Load nvm bash_completion
  if [ -f "$NVM_DIR/bash_completion" ]; then
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
  fi
}

for action in nvm node npm npx; do
  $action() {
    lazy_load_nvm
    $0 $@
  }
done
```

[//]: # (Link / Title)
[00]: https://til-engineering.nulogy.com/Slow-Terminal-Startup-Tip-Lazy-Load-NVM/ "Nulogy Engineering TIL, A microblog for web development"