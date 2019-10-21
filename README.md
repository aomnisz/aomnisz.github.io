# [AOMNISZ's BLOG](https://aomnisz.github.io/)

## What is this blog about ?

About nothing : )

Or everything : )

## How to run the blog locally ?

First, you need to install Ruby and Jekyll, see [Installation](https://jekyllrb.com/docs/installation/) for details.

Then, run the commands below:

```
git clone https://github.com/aomnisz/aomnisz.github.io.git
cd aomnisz.github.io/
bundle install --path vendor/bundle
bundle exec jekyll serve
```

## Why are the photos in assert/ directory in WebP format?

To protect the Earth, I convert all PNG images to WebP format. WebP lossless images are at least 50% smaller in size compared to PNGs. It can save a lot of disk space. However, WebP lossless images converted from JPG images are larger than their original files. Therefore, I use lossy compression on JPG images and keep the original files.
