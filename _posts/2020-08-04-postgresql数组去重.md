---
layout: post
title: "Postgresql数组去重"
author: Kehao Wu
tags:
 - postgresql
 - array
 - 去重
 - 数组
---



```
insert into demo (title, relkey) values ('Hello World', '{a}') 
  on conflict (title) do update set 
  relkey = array(select distinct unnest(array_cat(demo.relkey, excluded.relkey)));
```

