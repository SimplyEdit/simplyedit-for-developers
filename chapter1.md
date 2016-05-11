# Under the hood

Before we get started with a real plugin for SimplyEdit, it pays to get some feeling for how SimplyEdit works under the hood. If you already know how an inline WYSIWYG HTML editor works, you may be in for a surprise. SimplyEdit replaces many of the builtin browser features with its own version.

SimplyEdit is not just an editor, it also renders each page using a HTML template and a JSON dataset. So even if you are just reading a page, SimplyEdit is active. You can pre-render the page server-side, but it isn't needed.

If you take a look at the source of a page that uses SimplyEdit, the source code doesn't actually include the text you are reading. That text is retrieved by SimplyEdit from the JSON datastore and inserted in the page.

This is why SimplyEdit needs a `data-simply-endpoint`. This attribute contains a URL to the JSON data store.

The basic startup sequence for a SimplyEdit page is then as follows:

1. Load the HTML
2. Load simply-edit.js
3. Initialize SimplyEdit 
4. SimplyEdit loads the data from the JSON datastore
5. SimplyEdit parses the HTML. It looks for elements with `data-simply-\*`
6. For each `data-simply-field` element, SimplyEdit locates the content in the JSON data and inserts it into that element. It replaces existing content.
7. For each `data-simply-list` element, it will parse any `<template>` found inside. It will then add elements to the list for each entry in the JSON data that matches its name, using the matching template.
8. For each `data-simply-data` element, it will grab the data from the named data source instead of the default JSON data store. It will then render the list or field.

When you enter the edit mode, by adding `#simply-edit` to the URL, the followin happens:

1. SimplyEdit calls `connect()` on the JSON data store. If necessary this will open a login dialog. When connect() returns true, continue.
2. Locate all `data-simply-field` elements and make them editable. 
2. Locate all `data-simply-sortable` elements and make their contents sortable.
3. Show the main toolbar

