# MCForge

A Salesforce Marketing Cloud workbench that runs as a single CloudPage. It brings Data Extensions, Automation Studio, Email Studio and Content Builder together in one Lightning-style interface that also works on phones.

The whole tool is one file, `mcforge.html`. It holds a server-side JavaScript (SSJS) back end and an HTML/JS front end, and uses the Marketing Cloud REST and SOAP APIs through an Installed Package.

## Features

**Data Extensions**
- Create Data Extensions with a three-step wizard: properties and folder, data retention policy, and fields (including sendable and testable settings).
- Find Data Extensions, view their fields, rename them, move them to another folder, lengthen text fields and add fields.

**Automation Studio**
- Overview of automations by folder, with status, starting source, last run and schedule. Run Once and Delete from the list.
- Activities by type (SQL Query, Import, Script, Data Extract, File Transfer, Filter, Send Email): view, run, delete and create.
- Automation canvas with steps, activities and Wait activities, plus Save and Run Once with activity selection.
- Define a schedule (once, hourly, daily, weekly, monthly or yearly, with a time zone and an end condition) and activate or pause it.
- Live run status, run history and notification settings.

**Email Studio**
- Overview: send results for the last 30 days, recent sends and quick actions.
- Subscribers: All Subscribers (search, unsubscribe, reactivate, list memberships), lists (create, view members, add subscribers, delete) and sendable Data Extensions.
- Interactions: user-initiated emails (create, send, delete) and triggered emails (start, pause, publish changes).
- Tracking: sends with delivery, open, click, bounce and unsubscribe rates.
- Admin: send classifications, sender profiles and delivery profiles.

**Content Builder**
- Browse all content (emails, images, content blocks, templates and documents) in a collapsible folder tree.
- List and grid views, a type filter, sorting and page-by-page loading.
- Upload images and documents by drag and drop, up to 3 MB each.
- Preview, move and delete content, singly or in bulk. Create HTML emails and folders.

## Setup

1. **Create an API integration.** In Marketing Cloud Setup, go to **Apps > Installed Packages** and create a package with a **Server-to-Server** API integration. Grant the scopes for the features you use:
   - Data Extensions: read and write
   - Automations: read, write and execute
   - Email: read, write and send
   - Documents and Images, Saved Content: read and write
   - List and Subscribers: read and write
   - Tracking Events: read
2. **Add your credentials.** Open `mcforge.html` and fill in `CONFIG` near the top:
   ```js
   var CONFIG = {
     clientId:     "YOUR_CLIENT_ID",
     clientSecret: "YOUR_CLIENT_SECRET",
     subdomain:    "YOUR_SUBDOMAIN",  // from the authentication base URI
     accountId:    "YOUR_MID"         // business unit MID
   };
   ```
3. **Publish.** In **Web Studio > CloudPages**, create a page with a blank code view. Paste the whole file, then **Save** and **Publish**.
4. **Open the published URL.** The header shows **Connected - MID …** when the credentials work.

## Security

- **Never commit real credentials.** Keep `CONFIG` as placeholders in the repository and enter the values only in the CloudPage.
- **Restrict who can reach the page.** Anyone who can open the published URL can act on the business unit with the package's permissions. Protect the page, for example with CloudPage IP allowlisting or a login, or use it only in a test business unit.
- **Rotate the client secret** if it was ever shared or committed.

## Notes

- The front end and back end check that they are the same build (`MCFORGE-R24`). If the page reports a version mismatch, publish the complete file again and reload.
- Large lists load page by page so each request stays inside the CloudPage time limit.
- Actions that send email or change live data (Send, Start, Run Once, Delete) ask for confirmation first. Try them in a test business unit before using them in production.
