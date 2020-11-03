## vimwiki + jekyll + github pages

```sh
bundle install
bundle exec jekyll serve
```
### npm setup

```sh
npm install
```

### setup githook (pre-commit)

```sh
#!/bin/sh

./generateData.js
git add _data
git add data
```
