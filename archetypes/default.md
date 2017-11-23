---
title: "{{ replace .TranslationBaseName "-" " " | title }}"
date: {{ .Date }}
slug: {{ sha1 now.Unix }}
draft: true
---

