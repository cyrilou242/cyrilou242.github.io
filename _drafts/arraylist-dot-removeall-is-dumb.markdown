---
title: ArrayList.removeAll is dumb
date: 2024-02-27 11:42:00 Z
---

ArrayList.removeAll does not check if the collection is empty before looping through all the elements.