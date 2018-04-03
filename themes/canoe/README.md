# Canoe

A beautifull and powerfull theme of hugo static blog generator.   

[demo](https://keyin.me)   

![](https://raw.githubusercontent.com/stkevintan/canoe/master/images/screenshot.png)


## Feature

1. Waterfall layout
2. Responsive layout
3. Material design  
4. Table of content
5. Tiny and powerfull, no JQuery
5. Chinese keyword search(using `lunr` and `nodejieba`)  


## Install

```
cd /your-blog-path/theme
git clone https://github.com/stkevintan/canoe
```

then update your `config.toml` file with the help of the `exampleSite/config.toml`



refer to <https://gohugo.io/themes/> for more info.


## Enable Search

### The old way for search
~~My theme use [lunr](https://lunrjs.com/) to search keywords, so you need using [hugo-lunr](https://www.npmjs.com/package/hugo-lunr) to generate lunr index file every time after building your site. If you need Chinese support, use my tool:  [hugo-lunr-zh](https://www.npmjs.com/package/hugo-lunr-zh) instead. Plese refer to [official doc](https://gohugo.io/tools/search/#readout) for more info.~~

### A new way for search without any other dependencies but Hugo itself

As a fact, Hugo does have a way to generate `.json` format files of itself without any other dependencies. You can find it from [Hugo official doc](https://gohugo.io/functions/jsonify/#readout), and with some [variables](https://gohugo.io/variables/page/), you can generate a `index.json` file which looks like below:

```json
[
  {
  	"categories": ["A","B"],
    "content": "article content .........",
    "oriTitle": "article title",
    "permalink": "article url",
    "tags": ["tagA","tagB","tabC"],    
    "title": "file title"    
  }
];
```

Only two steps, you'll get it:

- **Step 1**:
	Creat a new file with the name `index.json`, and put it into the path `themes/you-theme-name/layouts/_default/index.json`. Set the content of `index.json` the same as below:

```json
{{- $.Scratch.Add "index" slice -}}
{{- range .Site.RegularPages -}}
    {{- $.Scratch.Add "index" (dict "oriTitle" .Params.title "title" .Title "tags" .Params.tags "categories" .Params.categories "content" .Plain "permalink" .Permalink) -}}
{{- end -}}
{{- $.Scratch.Get "index" | jsonify -}}
```

- **Step 2**:
	Add beloe content to your `config.toml`
```toml
[outputs]
  home = ["HTML", "RSS", "JSON"]
```

Now everytime you `hugo serve` or `hugo -b ""` , Hugo will generate a `index.json` file in your site root.


As `hugo-canoe-theme` already have it's own way to render `index.json` file for searching in `themes/canoe/static/js/index.js` , you won't have to do other things.


### Other hugo-theme enable search 
	
If you're using other hugo themes and want to enable search, you can refer to this `READEME` or [Client side searching for Hugo.io with Fuse.js](https://gist.github.com/eddiewebb/735feb48f50f0ddd65ae5606a1cb41ae) or [https://keyin.me/posts/hugo-guidance/](https://keyin.me/posts/hugo-guidance/)

### Summary

改完之后比较了一下，搜索能力与老方案比似乎还是差了一小点，应该可以从改善 `canoe` 内部的 `index.js` 出发得以解决，因为[Client side searching for Hugo.io with Fuse.js](https://gist.github.com/eddiewebb/735feb48f50f0ddd65ae5606a1cb41ae)方案的搜索能力比老方案似乎还强有一些。博客[https://kuleyu-hugo.netlify.com/](https://kuleyu-hugo.netlify.com/)就是采用了 `Fuse.js` 方案。


