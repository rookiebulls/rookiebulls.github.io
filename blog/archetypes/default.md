---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
slug: {{ .File.BaseFileName }}
categories:
  - 'Programming'
tags:
  - 'Python'
  - 'Javascript'
---
