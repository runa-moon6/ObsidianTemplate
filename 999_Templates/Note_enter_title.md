<%*
let title = await tp.system.prompt("Title");
await tp.file.rename(title + tp.date.now("YYYYMMDDHHmmss"));

tR += "---\n";
tR += "Title: " + title + "\n";
tR += "Priority: Mid\n";
tR += "Importance: Mid\n";
tR += "Created: " + tp.date.now("YYYY-MM-DD HH:mm") + ":00\n";
tR += "tags:\n";
tR += "  - " + title + "\n";
tR += "---";
%>
# 概要

# 