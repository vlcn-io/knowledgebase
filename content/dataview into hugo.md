---
title: "dataview into hugo"
---

https://github.com/blacksmithgu/obsidian-dataview/issues/42

Looks like people use [templater](https://github.com/SilentVoid13/Templater) to generate static markdown and refresh on occasion:

```
<%*  
const dv = app.plugins.plugins["dataview"].api;
const te = await dv.queryMarkdown(`TASK FROM #journal WHERE contains(text, "${tp.file.title}")`);
tR += te.value;
%>
```
