---
title: "Hugo の記事の更新情報を表示する"
date: 2018-02-04T09:22:10+09:00
slug: 6450b9f644bc710449e91a3fd29edcd03f800b27
draft: true
---
一度公開したブログの記事を更新したくなることがあるのですが、それがいつ更新されたのかが分からない。  
[Git Info Variables | Hugo](https://gohugo.io/variables/git/#lastmod) を見ると、最終更新日を表示できるので、公開日と一緒に表示するようにしてみます。  

テンプレートには以下の
```
<small class="pull-right">Last Updated on {{ .Lastmod.Format "Jan 2, 2006" }}</small>
```


それと、ここのブログは [GitHub qtkmz/blog](https://github.com/qtkmz/blog) で管理しているので、ファイルの更新履歴へのリンクも表示してみます。
GitHub リポジトリへの URL を config.toml に定義して、

```config.toml
[params]
content_commits_uri = "https://github.com/qtkmz/blog/commits/master/content"
```

テンプレートには次のように
```
<a href="{{ .Site.Params.content_commits_uri }}{{ .File.Path }}">updates</p>
```

