---
title: Random Password Generator
categories: linux
tags: linux bash programming
---

Quick script to generate random passwords

## Use Bash function (for learning purposes)
```bash
#!/bin/bash

# Function to generate a 32 character random password
randomPass() {

	#Set local variable for password
	local randompass=$(cat /dev/urandom | env LC_CTYPE=C tr -dc 'a-zA-Z0-9!@#$%^&*()_+?><~\`;' | fold -w 32 | head -n 1)

	echo $randompass
}

randomPass
```

## Quick one-liner

```bash
cat /dev/urandom | env LC_CTYPE=C tr -dc 'a-zA-Z0-9!@#$%^&*()_+?><~\`;' | fold -w 32 | head -n 1
```
