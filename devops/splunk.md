# Splunk Fundamentals

These are my notes for the official Splunk Fundamentals course provided for free on the Splunk website.

## Intro to Splunk

Apps are workspaces included in a Splunk instance.

There are three main user roles:
* Splunk administrators can install apps, ingest data, and create knowledge objects for all users.
* The power user can create and share knowledge objects for users of an app and do realtime searches.
* Regular users will only see their own knowledge obejcts and those shared with them.

Data summaries break down data by host, source, and sourcetype.

Searches include matching terms, contextual semantics, and syntax documentation. It is best practice to scope a search to a particular time period. Commands that create statistics and visualizations are called transforming commands. There are also options to pause, stop, share, or export search jobs. By default a search job will remain active for ten minutes. Shared search jobs remain active for seven days.

Search processing language can use boolean operators:
1. NOT
2. OR
3. AND

These are evaluated by order of priority.

Commands include search terms, commands, functions, arguments, and clauses (explain how results are grouped).

Example: sourcetype=db usage=True | stats count(usage) as Active
             Search Terms          Command Function(Argument)  Clause
             
Some best practices:
* Filter time
* More detailed queries provide better results
* Inclusion is better than exclusion for search patterns
* Use `OR` instead of wildcards
* Filter early in the command to speed up processing

Knowledge object types include fields, field extractions, calculated fields, event types, transactions, lookups, workflow actions, tags, field aliases, and data models.

Reports and dashboards can be used to save and share searches for future use or reference. Reports can be scoped to specific user level permissions.
