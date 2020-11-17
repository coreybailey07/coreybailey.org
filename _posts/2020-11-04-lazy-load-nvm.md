---
layout: post
title: Lazy Loading NVM
---

As with most developers experiencing sluggish shell initialization
post <code>nvm</code> install, it was proving a situation impacting
productivity. So after a few quick google queries I landed upon an
article by [evanbrodie][00] which pointed to a gist providing a clean
solution to the problem. For my purposes I implemented a slightly
dryer syntax by replacing the multiple function definitions with a
<code>for</code> loop. Lazy loading <code>nvm</code> with this method
cut shell initialization time down by half.

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