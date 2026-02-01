---
created: <% tp.date.now("YYYY-MM-DDTHH:mm") %>
updated: 2026-01-31T18:33
tags: []
---
<%*
let title = tp.file.title;
if (title.startsWith("Untitled")) {
    newTitle = await tp.system.prompt("Enter a title for the new file:");
    if (newTitle === "") {
	    newTitle = title;
    }
}
await tp.file.rename(newTitle);
%>