# My Collection of Jira Cloud Filters

By Jeremy Gagliardi │ As of 2025-08-22 | No license; these are free to copy and use.<br>
_The following are partial snippets from JSON export._<br>

Find what you need and copy these into the **Name**, **Description**, and **JQL** fields, respectively, to create your filter. To update the **Name** and **Description**, you need to click on the _Filter details_ button and then _Edit name and description_. To update the JQL code, you need to switch from _Basic_ to _JQL_, enter the JQL code from below, and press **Enter** to execute it. Don't forget to click **Save filter** when you're all done.<br>

 - Please note some organization-specific quirks in the following tables, which I swapped out for these generic terms. I kept them included, so you can see my examples to give you ideas for your own setup.
   - The following is a custom field JQL reference "our name for it" (what it contains)...
     - `cf[12345]` "Scheduled Maintenance" (date/timestamp)
   - The following is a (formerly Project) Space Key "Space Name"...
     - `AAR` "Automated Alert Response"
     - `CSD` "Customer Service Desk"
     - `INT` "Internal Issues"
     - `ENG` "Engineering Projects"
   - The following is a user display name (first/nickname) [Account ID reference]...
     - Additional notes...
       - When searching for yourself, you can use the built-in function `currentUser()`.
       - But when searching for other users, you can search for them by name, and in the filter interface, it'll show their profile badge and name, but in the actual JQL code, it stores them as account IDs only.
     - Samuel L. Jackson (Sam) [-some-account-id-string1-]
 - You'll also see references to status lists that may or may not match what your organization uses.
 - You'll need to customize these to your organization's keys, names, or IDs.
 - All newlines in the _Description_ or _JQL_ fields have been collapsed into spaces for proper table alignment presented here.
   - Jira lets you enter newlines in these fields by pressing Shift+Enter; you can only have up to 3 lines in the _JQL_ field.
 - The JQL clause `statusCategory != Done` is far superior to any `status = "whatever"`, because it'll work for every type of ticket, even when there are multiple _done_ statuses.

Here's a summary of all filters detailed below in the tables...
 - `All Open CSD Issues`
 - `All Open CSD with Blank Org`
 - `I'm @-mentioned all issues even done`
 - `I'm @-mentioned in status≠done`
 - `Latest updates any project/status/assignee`
 - `Open tickets assigned to Sam`
 - `Open tickets I assigned`
 - `⏳👉My tickets awaiting closure`
 - `⛔👉Mine in status≠done & blocked`
 - `👁️Watching others' tickets`
 - `👉My ACTIVE in last 24h`
 - `👉My ACTIVE tickets`
 - `👉My ticket history (assignee=me)`
 - `👉My tickets in status≠done`
 - `👉⛔Mine in status≠done blocking others`
 - `👉🆕My NEW tickets all`
 - `👉📅My work that is Scheduled out`
 - `📥All check sheets in status≠done`
 - `🔥🆕LATEST CUSTOMER UPDATES`
 - `🔥🆕UNASSIGNED AAR/CSD/INT`
 - `🔥🆕👉My NEW unviewed tickets`
 - `🖇️I'm Rptr/Prtcpt/@ment'd assignee≠me`

Now, the detailed tables...<br>
<br>
| FIELD | VALUE |
|---|---|
| **Name** | `All Open CSD Issues` |
| **Description** | `All tickets where project is CSD and status is not done.` |
| **JQL** | `project = "Customer Service Desk" AND statusCategory != Done ORDER BY priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `All Open CSD with Blank Org` |
| **Description** | `All tickets where the project is CSD, status is not done, and the organization field is blank.` |
| **JQL** | `project = "Customer Service Desk" AND statusCategory != Done AND Organizations = EMPTY ORDER BY priority DESC, updated DESC` |
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
| **Name** | `I'm @-mentioned in status≠done` |
| **Description** | `I'm @-mentioned in a comment, and the status is not done (Use "I'm @-mentioned all issues even done" otherwise).` |
| **JQL** | `statusCategory != Done AND comment ~ currentUser() ORDER BY updated DESC` |
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
| **Name** | `Open tickets assigned to Sam` |
| **Description** | `All tickets in status not done and assignee is Samuel L. Jackson.` |
| **JQL** | `statusCategory != Done AND assignee = -some-account-id-string1- ORDER BY priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `Open tickets I assigned` |
| **Description** | `Tickets where status is not done, I'm the reporter, but somebody else is the assignee.` |
| **JQL** | `statusCategory != Done AND reporter = currentUser() AND assignee != currentUser() ORDER BY assignee ASC, priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `⏳👉My tickets awaiting closure` |
| **Description** | `All tickets where I'm the current assignee with a status of Resolved, not Closed.  This is necessary because the time-based ticket closure automation broke a while ago, and it just leaves tickets in the Resolved status until someone notices.` |
| **JQL** | `assignee = currentUser() AND status = Resolved ORDER BY updated DESC` |
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
| **Name** | `👁️Watching others' tickets` |
| **Description** | `All tickets where I'm listed as a watcher and status is not done.  It excludes tickets that are already assigned to me (use "My tickets in status≠done" for that).  To clean up your watched list, simply load the ticket, click the 👁 icon, and select Stop watching, or just press "w" to toggle it back & forth.` |
| **JQL** | `watcher = currentUser() AND assignee != currentUser() AND statusCategory != Done ORDER BY priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `👉My ACTIVE in last 24h` |
| **Description** | `All tickets updated in the past 24 hours where I'm the current assignee and it's in a status that is active (or more accurately not one of the inactive/waiting statuses).  Complies with all project workflows as of 02/08/2025.` |
| **JQL** | `assignee = currentUser() AND project IN (AAR,CSD,INT,ENG,ICS) AND updated >= -24h AND status in ("Alert Reported","Awaiting Manager Signature",Discovery,"In Progress","Investigation In Progress","New Request",Open,"Package Received","Request in Progress","To Do","Visit in Progress") ORDER BY updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `👉My ACTIVE tickets` |
| **Description** | `All tickets where I'm the current assignee and it's in a status that is active (or more accurately not one of the inactive/waiting statuses).  Complies with all project workflows as of 02/08/2025.` |
| **JQL** | `assignee = currentUser() AND status in ("Alert Reported","Awaiting Manager Signature",Discovery,"In Progress","Investigation In Progress","New Request",Open,"Package Received","Request in Progress","To Do","Visit in Progress") ORDER BY priority DESC, updated DESC` |
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
| **Name** | `👉My tickets in status≠done` |
| **Description** | `All tickets where I'm the current assignee and status is not done.` |
| **JQL** | `assignee = currentUser() AND statusCategory != Done ORDER BY priority DESC, updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `👉⛔Mine in status≠done blocking others` |
| **Description** | `All tickets where I'm the current assignee, status is not done, and my tickets indicate they're blocking others. However, it's not possible with JQL to filter out blocking issues that are themselves done (thus not blocking anymore), only tickets that have links indicating "is blocked by" other tickets, so it's very possible that tickets will show up in this queue even if all of the blockers have been done already.` |
| **JQL** | `assignee = currentUser() AND issueLinkType = blocks AND statusCategory != Done ORDER BY priority DESC, updated DESC` |
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
| **Name** | `👉📅My work that is Scheduled out` |
| **Description** | `All tickets assigned to me with a status of "Scheduled out"` |
| **JQL** | `assignee = currentUser() AND status = "Scheduled out" ORDER BY cf[12345] ASC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `📥All check sheets in status≠done` |
| **Description** | `All Internal Check Sheets tickets that are not done. Complies with workflow as of 02/01/2025.` |
| **JQL** | `Project = "Internal Check Sheets" AND statusCategory != Done ORDER BY created DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `🔥🆕LATEST CUSTOMER UPDATES` |
| **Description** | `Simply shows all tickets in project CSD, regardless of status or assignee, ordered by updated descending.` |
| **JQL** | `project = "Customer Service Desk" ORDER BY updated DESC` |
<br>
<br>

| FIELD | VALUE |
|---|---|
| **Name** | `🔥🆕UNASSIGNED AAR/CSD/INT` |
| **Description** | `All unresolved tickets with no assignee.  Includes only Automated Alerts, Customer Service Desk, & Internal Issues.` |
| **JQL** | `assignee IS EMPTY AND project IN (AAR, INT, CSD) AND statusCategory != Done ORDER BY priority DESC, created ASC` |
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
| **Name** | `🖇️I'm Rptr/Prtcpt/@ment'd assignee≠me` |
| **Description** | `All tickets with a status not done where I'm a Reporter, a Requested participant, or @-mentioned in a comment, but I'm not the assignee.` |
| **JQL** | `statusCategory != Done AND assignee != currentUser() AND ( reporter = currentUser() OR "request participants" = currentUser() OR comment ~ currentUser()) ORDER BY priority DESC, updated DESC` |
<br>
<br>
