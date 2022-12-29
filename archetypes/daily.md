---
title: "{{ replace .ContentBaseName "_" " " | title }}"
date: {{ .Date }}
categories:
- Daily
tags:
- tag1
- tag2

draft: false
publishdate: {{ time.Format "2006-01-02T15:04:00-07:00" (time ((.Date | time.AsTime).AddDate 0 0 1 | time.Format "2006-01-02") "Asia/Taipei") }}

# gallery:
#   - 

# summary: 
# thumbnailImage: {{ replace .File.Dir "\\" "/" }}
---