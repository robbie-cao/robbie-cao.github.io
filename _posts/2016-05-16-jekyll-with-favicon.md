---
layout: post
categories: markup
---

> How to setup favicon on Jekyll page.

## Convert Image to Icon

- From JPG/GIF/PNG to ICO with `convert`

```
$ /usr/bin/convert -resize x16 -gravity center -crop 16x16+0+0 input.jpg \
-transparent white -colors 256 output/favicon.ico

$ /usr/bin/convert -resize x16 -gravity center -crop 16x16+0+0 input.png \
-flatten -colors 256 output/favicon.ico
```

- From JPG/GIF/PNG to ICO with `ffmpeg`

```
$ ffmpeg -i img.png img.ico
```

## Embed Favicon into Page

- Home Page (index.html)

```
<link rel="shortcut icon" type="image/png" href="/favicon.png">
```

- Post Page (_post/yyyy-mm-dd-xxx.md)

```
<link rel="shortcut icon" type="image/png" href="{{ "/favicon.png" | prepend: site.baseurl }}">
```

## Issues

The following code does NOT work, reason unknown.

```
<link rel="shortcut icon" type="image/x-icon" href="/favicon.ico">

<link rel="shortcut icon" type="image/x-icon" href="/favicon.ico?">

<link rel="shortcut icon" type="image/x-icon" href="{{ "/favicon.ico" | prepend: site.baseurl }}">

<link rel="shortcut icon" type="image/x-icon" href="{{ site.baseurl }}/images/favicon.ico">
```

## Reference

- [Favicon - Wiki](https://en.wikipedia.org/wiki/Favicon)
- [Converting GIF's, PNG's and JPG's to .ICO files using Imagemagick](http://stackoverflow.com/questions/3185677/converting-gifs-pngs-and-jpgs-to-ico-files-using-imagemagick)
- [Unable to set favicon using Jekyll and github pages](http://stackoverflow.com/questions/30551501/unable-to-set-favicon-using-jekyll-and-github-pages)

