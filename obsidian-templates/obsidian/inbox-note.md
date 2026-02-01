---
created: <% tp.date.now("YYYY-MM-DDTHH:mm") %>
updated: 2026-02-01T02:28
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