+++
title = "{{ replace .Name "-" " " | title }}"
date = "{{ dateFormat "2006-01-02" .Date }}"
categories = ["tag1","tag2"]
type = ["posts","post"]
[ author ]
  name = "Diego Moreno"
draft = true
+++