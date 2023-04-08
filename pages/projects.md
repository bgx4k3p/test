---
layout: page
menu: false
date: '2020-02-27 01:53:59'
title: About
description: Projects.
permalink: /projects/
---
{% for repo in site.github.public_repositories %}

{% if repo.fork == false %}

[{{ repo.name }}]({{ repo.html_url }})

{{ repo.description }}

Last updated: {{ repo.updated_at | date_to_string }}

{% endif %}

{% endfor %}