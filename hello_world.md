# Hello World

This chapter shows how to create a very simple plugin that adds a button to the main toolbar. When pressed it will show a dialog with 'Hello World'.

First make a new HTML file for the hello world plugin, called 'hello-world.html':

```
<section id="hello-world" class="simply-dialog simply-modal">
    <h1>Hello World</h1>
</section>
<style>
  #hello-world h1 {
    font-size: 40px;
  }
</style>
<script>
  function makeButton(action, icon, text) {
    var listItem = document.createElement("li");
    var button   = document.createElement("button");
    var icon     = document.createElement("i");
    icon.classList.add('fa');
    icon.classList.add(icon);
    button.dataset.simplyAction = action;
    button.appendChild(icon);
    button.appendChild(document.createTextNode(text));
    listItem.appendChild(button);
    return listItem;
  }
  
  editor.plugins.helloWorld = {
    dialog: {
      open: function() {
        editor.plugins.dialog.open(document.getElementById('hello-world'));
      }
    },
    init: function(config) {
      var button = makeButton('hello-world', 'fa-globe', 'Hello');
      
      document
      .querySelector("#simply-main-toolbar .simply-buttons")
      .appendChild(button);
      
      editor.addAction("hello-world", editor.plugins.helloWorld.dialog.open);
    }
  }
  editor.plugins.helloWorld.init();
</script>
```

This is your plugin. Now we need to add it to SimplyEdit. So open a template or page where you're using SimplyEdit. Somewhere near the bottom should be a script tag like this:

```
<script src="https://beta.simplyedit.io/0/simply-edit.js" 
		data-api-key="example"> 
```

Immediately after this script tag add the following:

```
<script>
  editor.loadToolbar('hello-world.html');
</script>
```

If you reload the page and start SimplyEdit, you will see your new Hello button in the main toolbar.
When you press it a dialog opens with 'Hello World'. 

However, you can't close the dialog yet. For that, you need to add a toolbar in the dialog and add a close button to it. So open hellow-world.html and change the dialog html code like this:

```
<section id="hello-world" class="simply-dialog simply-modal">
  <div class="simply-toolbar">
    <ul class="simply-buttons">
      <li class="simply-right">
        <button data-simply-action="simply-dialog-close">
          <i class="fa fa-times"></i>Close
        </button>
      </li>
    </ul>
  </div>
  <div class="simply-toolbar-section simply-selected">
    <h1>Hello World</h1>
  </div>
</section>
```

Now reload the page, press 'Hello' and the dialog has a toolbar with a close button.