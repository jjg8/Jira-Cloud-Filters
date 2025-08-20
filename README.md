# Some of my more useful Jira Cloud Filters

By Jeremy Gagliardi │ As of 2025-08-19 │ No license.<br>
_The following are partial snippets from JSON._<br>
Find what you need and copy these into the **Name**, **Description**, and **JQL** fields, respectively, to create your filter.<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `👉My tickets in status≠done` |
| **description** | `All tickets where I'm the current assignee and status is not done.` |
| **jql** | `assignee = currentUser()`<br>`AND statusCategory != Done`<br>`ORDER BY priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `🖇️I'm Rptr/Prtcpt/@ment'd assignee≠me` |
| **description** | `All tickets with a status not done where I'm a Reporter, a Requested participant, or @-mentioned in a comment, but I'm not the assignee.` |
| **jql** | `statusCategory != Done AND assignee != currentUser()`<br>`AND ( reporter = currentUser() OR "request participants" = currentUser() OR comment ~ currentUser())`<br>`ORDER BY priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `⛔👉Mine in status≠done & blocked` |
| **description** | `All tickets where I'm the current assignee, status is not done, and other tickets indicate they're blocking these.  However, it's not possible with JQL to filter out blocking issues that are themselves done (thus not blocking anymore), only tickets that have links indicating "is blocked by" other tickets, so it's very possible that tickets will show up in this queue even if all of the blockers have been done already.` |
| **jql** | `assignee = currentUser() AND statusCategory != Done AND issueLinkType = "is blocked by"`<br>`ORDER BY priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `⏳👉My tickets awaiting closure` |
| **description** | `All tickets where I'm the current assignee with a status of Resolved, not Closed.  This is necessary because the time-based ticket closure automation broke a while ago, and it just leaves tickets in the Resolved status until someone notices.` |
| **jql** | `assignee = currentUser() AND status = Resolved` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `🔥🆕👉My NEW unviewed tickets` |
| **description** | `All tickets where I'm the current assignee, the status is not done, but I haven't viewed the ticket yet.  NOTE, even if you've viewed the ticket before, if you haven't viewed it in a while (~18 days), it may show back up in this queue again.  Viewing the ticket even once removes it from this queue.  To view all your new tickets, even if you've already viewed them recently, use "My NEW tickets all".` |
| **jql** | `assignee = currentUser() AND lastViewed IS EMPTY`<br>`AND statusCategory != Done`<br>`ORDER BY priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `I'm @-mentioned in status≠done` |
| **description** | `I'm @-mentioned in a comment, and the status is not done (Use "I'm @-mentioned all issues even done" otherwise).` |
| **jql** | `statusCategory != Done AND comment ~ currentUser() ORDER BY updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `I'm @-mentioned all issues even done` |
| **description** | `I'm @-mentioned in a comment in all issues, even when status is done (Use "I'm @-mentioned in status≠done" otherwise).` |
| **jql** | `comment ~ currentUser() ORDER BY updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `️👁️Watching others' tickets` |
| **description** | `All tickets where I'm listed as a watcher and status is not done.  It excludes tickets that are already assigned to me (use "My tickets in status≠done" for that).  To clean up your watched list, simply load the ticket, click the 👁 icon, and select Stop watching, or just press "w" to toggle it back & forth.` |
| **jql** | `watcher = currentUser() AND assignee != currentUser() AND statusCategory != Done` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `👉🆕My NEW tickets all` |
| **description** | `All tickets where I'm the current assignee, and it has no status yet (i.e. freshly created), whether I've viewed it recently or not.  To see only tickets I haven't viewed recently, use "My NEW unviewed tickets".  Also, if someone sets the status, it drops off this filter.` |
| **jql** | `assignee = currentUser() AND status = EMPTY`<br>`AND statusCategory != Done`<br>`ORDER BY priority DESC, created ASC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `Latest updates any project/status/assignee` |
| **description** | `Simply shows all tickets (regardless of status or assignee) ordered by updated descending.` |
| **jql** | `ORDER BY updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `👉My ticket history (assignee=me)` |
| **description** | `All tickets where I'm the current assignee, with any status.` |
| **jql** | `assignee = currentUser()`<br>`ORDER BY priority DESC, created ASC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `👉⛔Mine in status≠done blocking others` |
| **description** | `All tickets where I'm the current assignee, status is not done, and my tickets indicate they're blocking others. However, it's not possible with JQL to filter out blocking issues that are themselves done (thus not blocking anymore), only tickets that have links indicating "is blocked by" other tickets, so it's very possible that tickets will show up in this queue even if all of the blockers have been done already.` |
| **jql** | `assignee = currentUser() AND issueLinkType = blocks AND statusCategory != Done` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **name** | `Open tickets I assigned` |
| **description** | `Tickets where status is not done, I'm the reporter, but somebody else is the assignee.` |
| **jql** | `statusCategory != Done AND reporter = currentUser() AND assignee != currentUser()` |
<br>
<br>

