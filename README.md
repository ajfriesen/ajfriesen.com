# [Personal Website](https://sourcethemes.com/academic/)

![github pages](https://github.com/ajfriesen/ajfriesen.com/workflows/github%20pages/badge.svg)

## Used Tech

* [Hugo](https://gohugo.io/)
* [Academic Theme](https://sourcethemes.com/academic/)
* Github Pages
* Github Actions

## Hugo Acedmic version

**v4.8.0**

## Get latest git submodule

When switching devices and meanwhile I have updated to a newer academic version

```
git submodule update --init --recursive
```

## Update instructions to update academic theme

https://sourcethemes.com/academic/docs/update/#if-you-installed-academic-kickstart

```
git submodule update --remote --merge
cd themes/academic
git checkout <VERSION>

```

## Add post

[Create a blog post](https://sourcethemes.com/academic/docs/managing-content/#create-a-blog-post)

```
hugo new  --kind post post/my-article-name
```