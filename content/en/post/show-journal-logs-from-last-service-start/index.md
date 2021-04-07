---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Show jounrald logs from last service start"
subtitle: ""
summary: ""
authors: [admin]
tags: ["systemd", "logs", "journald"]
categories: []
date: 2021-03-04T14:56:52+01:00
lastmod: 2021-03-04T14:56:52+01:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: "[systemd logo](https://brand.systemd.io/)"
  focal_point: "Center"
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

I needed to test something and was only interested in the logs since the last start/restart of a service.
For this you can use the **systemd invocation id** from a systemd service.

<!--more-->

You can check the current systemd invocation id like so:

```bash
systemctl show -p InvocationID --value kubelet.service
```

Then just use the `_SYSTEMD_INVOCATION_ID` option in `journalctl` to only see logs from the last service start:

```bash
journalctl -u kubelet.service _SYSTEMD_INVOCATION_ID="$(systemctl show -p InvocationID --value kubelet.service)"
```

Have fun!
