---
title: "{{ replace .Name "-" " " | title }}" # Add a title
description: "Add your description" # Add a description
date: {{ .Date }}
draft: true # Make it "true" if you don't want Hugo to "publish" yet
author: "Dana" # Add your name
tags: ["tag1", "tag2"] # Add some tags
---