---
title: Linux
linktitle: Linux
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  example:
    parent: 
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

## Memory Managemnt

A really good talk about Memory Management in Linux 
"[SREcon19 Asia/Pacific - Linux Memory Management at Scale: Under the Hood](https://www.youtube.com/watch?v=beefUhRH5lU&feature=youtu.be)":

{{< youtube beefUhRH5lU >}}

Learned a lot:
- RSS is only measured because it is easy
- It is not easy to check if your memory is overalocated. It could also be just used very efficiently
- cgroupsv1 vs cgroupv2
- SWAP is good to free up memory which is not backed up by anything on the disk. (anon memory)
- [In defence of swap: common misconceptions](https://chrisdown.name/2018/01/02/in-defence-of-swap.html)
- [Linux ate my ram! Don't Panic! Your ram is fine!](https://www.linuxatemyram.com/)

Userspace oom software:

- [oomd by facebook](https://github.com/facebookincubator/oomd)
- [earlyoom](https://github.com/rfjakob/earlyoom)

## Finding a package for every distribution

Ever wanted to know how to install `telet` in alpine but forgot the package name for that distribution?
This website can help!

https://command-not-found.com/

## A really nice shell

https://ohmyz.sh/

I use this with the [Powerlevel10k Theme](https://github.com/romkatv/powerlevel10k)


## Poor mans monitoring

[atop](https://www.atoptool.nl/)
