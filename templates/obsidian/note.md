---
created: <% tp.date.now("YYYY-MM-DDTHH:mm") %>
updated: <% tp.date.now("YYYY-MM-DDTHH:mm") %>
tags: []
---
<%*
let title = tp.file.title;
if (title.startsWith("Untitled")) {
    title = await tp.system.prompt("Enter a title for the new file:");
}
await tp.file.rename(title);
%>