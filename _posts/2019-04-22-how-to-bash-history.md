---
title: Dedup BASH History
categories: linux
tags: linux bash macos how-to
---

Script to dedup Bash History

```bash
# Remove duplicates
nl ~/.bash_history | sort -k 2  -k 1,1nr| uniq -f 1 | sort -n | cut -f 2 > unduped_history

# Replace history file
cp unduped_history ~/.bash_history
rm -f unduped_history
```
