---
created: <% tp.date.now("YYYY-MM-DDTHH:mm") %>
updated: 2026-01-31T17:06
tags: []
---
<%*
let title = tp.file.title;
if (title.startsWith("Untitled")) {
    title = await tp.system.prompt("Enter a title for the new file:");
    if (!title) {
	    await tp.file.rename(tp.date.now("YYYY-MM-DD_HH-mm-ss"));
    }
}
%>