---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Decrypt and Mount LUKS Volume in Grub Rescue Mode"
subtitle: "How to boot when you entered the wrong password in grub for LUKS"
summary: ""
authors: [Andrej Friesen]
tags: [grub, luks]
categories: []
date: 2020-04-13T01:26:15+02:00
lastmod: 2020-04-13T01:26:15+02:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

I am running arch linux on my server with LUKS disc encryption.
I do need to type in my password every time I reboot the machine.
Sometimes I have a typo and this happens:

```
Welcome to GRUB!

Attempting to decrypt master key...
Enter passphrase for hd1,gpt2 (<disk uuid>):
error: access denied
error: no such cryptodisk found.
error: disk `cryptouuid/<disk uuid>` not found.
Entering rescue mode...
```

Since with IPMI I do not need to attach a VGA cable and keyboard to my server but still the grub rescue console is something I am not familiar with and need to deal with.

You can mount the partition with `cryptomount`:[^1]

```
cryptomount (hd1,gpt2)
```

Enter your password.

Then load the module for a normal boot

```
insmod normal
```

Boot:

```
normal
```

Now you shoul get forwarded to the grub boot manager as you typed the password correct the first time.

[^1]: [cryptomount](https://www.gnu.org/software/grub/manual/grub/html_node/cryptomount.html#cryptomount)