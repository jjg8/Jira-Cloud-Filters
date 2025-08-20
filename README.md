# Some of my more useful Jira Cloud Filters

By Jeremy Gagliardi │ As of 2025-08-19 │ No license; free to copy and use.<br>
_The following are partial snippets from JSON._<br>
Find what you need and copy these into the **Name**, **Description**, and **JQL** fields, respectively, to create your filter.<br>
To update the **Name** and **Description**, you need to click on the _Filter details_ button and then _Edit name and description_.<br>
To update the JQL code, you need to switch from _Basic_ to _JQL_, enter the JQL code below, and press **Enter** to execute it.<br>
Don't forget to click **Save filter** when you're all done.<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `👉My tickets in status≠done` |
| **Description** | `All tickets where I'm the current assignee and status is not done.` |
| **JQL** | `assignee = currentUser() AND statusCategory != Done ORDER BY priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `🖇️I'm Rptr/Prtcpt/@ment'd assignee≠me` |
| **Description** | `All tickets with a status not done where I'm a Reporter, a Requested participant, or @-mentioned in a comment, but I'm not the assignee.` |
| **JQL** | `statusCategory != Done AND assignee != currentUser() AND ( reporter = currentUser() OR "request participants" = currentUser() OR comment ~ currentUser()) ORDER BY priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `⛔👉Mine in status≠done & blocked` |
| **Description** | `All tickets where I'm the current assignee, status is not done, and other tickets indicate they're blocking these.  However, it's not possible with JQL to filter out blocking issues that are themselves done (thus not blocking anymore), only tickets that have links indicating "is blocked by" other tickets, so it's very possible that tickets will show up in this queue even if all of the blockers have been done already.` |
| **JQL** | `assignee = currentUser() AND statusCategory != Done AND issueLinkType = "is blocked by" ORDER BY priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `⏳👉My tickets awaiting closure` |
| **Description** | `All tickets where I'm the current assignee with a status of Resolved, not Closed.  This is necessary because the time-based ticket closure automation broke a while ago, and it just leaves tickets in the Resolved status until someone notices.` |
| **JQL** | `assignee = currentUser() AND status = Resolved` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `🔥🆕👉My NEW unviewed tickets` |
| **Description** | `All tickets where I'm the current assignee, the status is not done, but I haven't viewed the ticket yet.  NOTE, even if you've viewed the ticket before, if you haven't viewed it in a while (~18 days), it may show back up in this queue again.  Viewing the ticket even once removes it from this queue.  To view all your new tickets, even if you've already viewed them recently, use "My NEW tickets all".` |
| **JQL** | `assignee = currentUser() AND lastViewed IS EMPTY AND statusCategory != Done ORDER BY priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `I'm @-mentioned in status≠done` |
| **Description** | `I'm @-mentioned in a comment, and the status is not done (Use "I'm @-mentioned all issues even done" otherwise).` |
| **JQL** | `statusCategory != Done AND comment ~ currentUser() ORDER BY updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `I'm @-mentioned all issues even done` |
| **Description** | `I'm @-mentioned in a comment in all issues, even when status is done (Use "I'm @-mentioned in status≠done" otherwise).` |
| **JQL** | `comment ~ currentUser() ORDER BY updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `️👁️Watching others' tickets` |
| **Description** | `All tickets where I'm listed as a watcher and status is not done.  It excludes tickets that are already assigned to me (use "My tickets in status≠done" for that).  To clean up your watched list, simply load the ticket, click the 👁 icon, and select Stop watching, or just press "w" to toggle it back & forth.` |
| **JQL** | `watcher = currentUser() AND assignee != currentUser() AND statusCategory != Done` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `👉🆕My NEW tickets all` |
| **Description** | `All tickets where I'm the current assignee, and it has no status yet (i.e. freshly created), whether I've viewed it recently or not.  To see only tickets I haven't viewed recently, use "My NEW unviewed tickets".  Also, if someone sets the status, it drops off this filter.` |
| **JQL** | `assignee = currentUser() AND status = EMPTY AND statusCategory != Done ORDER BY priority DESC, created ASC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `Latest updates any project/status/assignee` |
| **Description** | `Simply shows all tickets (regardless of status or assignee) ordered by updated descending.` |
| **JQL** | `ORDER BY updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `👉My ticket history (assignee=me)` |
| **Description** | `All tickets where I'm the current assignee, with any status.` |
| **JQL** | `assignee = currentUser() ORDER BY priority DESC, created ASC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `👉⛔Mine in status≠done blocking others` |
| **Description** | `All tickets where I'm the current assignee, status is not done, and my tickets indicate they're blocking others. However, it's not possible with JQL to filter out blocking issues that are themselves done (thus not blocking anymore), only tickets that have links indicating "is blocked by" other tickets, so it's very possible that tickets will show up in this queue even if all of the blockers have been done already.` |
| **JQL** | `assignee = currentUser() AND issueLinkType = blocks AND statusCategory != Done` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `Open tickets I assigned` |
| **Description** | `Tickets where status is not done, I'm the reporter, but somebody else is the assignee.` |
| **JQL** | `statusCategory != Done AND reporter = currentUser() AND assignee != currentUser()` |
<br>
<br>
