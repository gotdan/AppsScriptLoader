# Load Scripts from Google Drive

The excellent syncing built into Google Drive makes it easy to create javascript files on your computer and run them within Google Apps. This approach means you can use your favorite text editor rather then the online code editor, but still easily test in Google Apps without needing to copy and paste repeatedly.

You can even write code in [coffeescript](http://www.coffeescript.org) and have it automatically compile to javascript and sync to the cloud, ready to test. Finally, although Google recently introduced [Libraries](https://developers.google.com/apps-script/guide_libraries) in Google Apps, this approach represents a simple way to share code between projects while still supporting granular permissions.

To get started:
1. Set up [Google Drive syncing](http://support.google.com/drive/bin/answer.py?hl=en&answer=2374983) on your development computer.

2. Create a new javascript or coffeescript file in your Google Drive folder (coffeescript folks may want to set up an [automatic compilation watcher](http://coffeescript.org/#usage) on that folder).

3. Open your file in the online Google Drive. It will show in a document viewer, but the important thing to find is the document's id. This shows up in the url: https://docs.google.com/file/d/{DOCUMENT ID}/edit
.

4. Create a new [Google Apps](https://developers.google.com/apps-script/execution_methods#web_app) application and paste in the loader code below. Be sure to customize it for the document id you found in step 3. Also, if your code requires any permissions, you'll have to reference them here since Apps Script isn't able to find them in a loaded file.

```javascript
function doGet() {
  loadFile('DOCUMENT ID');
  //your app entry here
}

function bootstrap() {
  //reference permissions you might need
  Browser.msgBox('');
}

function loadFile(fileId) {
  var fileContent = DocsList.getFileById(fileId).getContentAsString();
  try {
    eval(fileContent);
  } catch (e) {
    var fileName = DocsList.getFileById(fileId).getName();
    e.message = [
      "fileName", "line " + e.lineNumber, e.message
    ].join(" | ");
    throw e;
  }
}
```

That's it! If you distribute your application to other users (for example, by including it within a shared spreadsheet) remember to set the sharing permissions on the javascript files you are loading or a folder containing both the application and javascript files.

You may also be interested in my open source [Apps Script user interface library](https://github.com/gotdan/uibot).