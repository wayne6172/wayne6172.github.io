---
title: "{{ replace .TranslationBaseName "-" " " | title }}"
date: {{ .Date }}
categories:
- category
- subcategory
tags:
- tag1
- tag2
keywords:
- tech

draft: false

publishdate: {{ time.Format "2006-01-02T15:04:00-07:00" (time ((.Date | time.AsTime).AddDate 0 0 1 | time.Format "2006-01-02") "Asia/Taipei") }}
# summary: 無
# thumbnailImage: //example.com/image.jpg
# thumbnailImagePosition: right
---

