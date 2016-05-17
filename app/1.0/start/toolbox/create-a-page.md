---
title: Step 2. Create a new page
subtitle: "Build your first Polymer application"
---

<!-- toc -->

The template we started with came with 3 placeholder pages you might use
start building out the views of your application.  At some point you may
wish to add additional pages, so let's walk through that process.

## Create an element for the new page

First you can create a new custom element that encapsulates the contents of
your new view.

1. Create a new file called `src/my-newview.html` and open it in an editor.

2. Add some scaffolding for a new custom element definition using Polymer:

    ```
    <link rel="import" href="../bower_components/polymer/polymer.html">

    <dom-module id="my-newview">

      <!-- Defines the element's style and local DOM -->
      <template>
        <style>
          :host {
            display: block;
            padding: 16px;
          }
        </style>
        <h1>New view</h1>
      </template>

      <!-- Creates the element's prototype and registers it -->
      <script>
        Polymer({
          is: 'my-newview',
          properties: {
            route: Object
          }
        });
      </script>

    </dom-module>

    ```

For now our element is very basic, and just has a `<h1>` that says "New View",
but we can return to it and make it more interesting later.

## Add the element to your app

1.  Open `src/my-app.html` in a text editor.

1.  Find the set of existing pages inside the `<iron-pages>`:

    ```
    ...
      <iron-pages role="main" selected="[[page]]" attr-for-selected="name" selected-attribute="visible">
        <my-view1 name="view1" route="[[subroute]]"></my-view1>
        <my-view2 name="view2" route="[[subroute]]"></my-view2>
        <my-view3 name="view3" route="[[subroute]]"></my-view3>
      </iron-pages>
    ...
    ```

    The `<iron-pages>` is bound to the `page` variable that changes with the
    route, and selectes the active page while hiding the others.

1.  Add your new page inside the iron-pages:

    ```
        <my-newview name="newview" route="[[subroute]]"></my-newview>
    ```

    Your `<iron-pages>` should now look like this:

    ```
    ...
      <iron-pages role="main" selected="[[page]]" attr-for-selected="name" selected-attribute="visible">
        <my-view1 name="view1" route="[[subroute]]"></my-view1>
        <my-view2 name="view2" route="[[subroute]]"></my-view2>
        <my-view3 name="view3" route="[[subroute]]"></my-view3>
        <my-newview name="newview" route="[[subroute]]"></my-newview>
      </iron-pages>
    ...
    ```

    Note: Normally when using a new custom element for the first time, you'd
    want to add an HTML Import to ensure the component definition has been
    loaded.  However, this app template is already set up to lazy-load top
    level views on-demand based on the route, so in this case we won't
    add an import for our new `<my-newview>` element.

    The following code that came with the app template will ensure the
    definition for each page has been loaded when the route changes.  As
    you can see, we followed the simple `'my-' + page + '.html'` scheme
    for loading the view (and you can adopt this simple scheme as you
    like to handle more complex routing and lazy loading).

    ```
      _pageChanged: function(page) {
        if (page != null) {
          // load page import on demand.
          this.importHref(
            this.resolveUrl('my-' + page + '.html'), null, null, true);
        }
      }
    ```

## Create a navigation menu item

Last, we'll add a menu item in the left-hand drawer to allow navigating to
our new page.

1.  Keep `src/my-app.html` open in your editor.

1.  Find the navigation menu inside the `<app-drawer>` element.

    ```
    ...
    <!-- Drawer content -->
    <app-drawer>
      <app-toolbar>Menu</app-toolbar>
      <iron-selector role="navigation" class="drawer-list" attr-for-selected="name" selected="[[page]]">
        <a name="view1" href="/view1">View One</a>
        <a name="view2" href="/view2">View Two</a>
        <a name="view3" href="/view3">View Three</a>
      </iron-selector>
    </app-drawer>
    ...
    ```

    Each navigation menu item consists of an anchor element (`<a>`) styled with CSS.

1.  Add the following new navigation item to the bottom of the menu.

    ```
        <a name="newview" href="/newview">New View</a>
    ```

    Your menu should now look like the following:

    ```
    ...
    <!-- Drawer content -->
    <app-drawer>
      <app-toolbar>Menu</app-toolbar>
      <iron-selector role="navigation" class="drawer-list" attr-for-selected="name" selected="[[page]]">
        <a name="view1" href="/view1">View One</a>
        <a name="view2" href="/view2">View Two</a>
        <a name="view3" href="/view3">View Three</a>
        <a name="newview" href="/newview">New View</a>
      </iron-selector>
    </app-drawer>
    ...
    ```

Your new page is now ready! Open your web browser and view it at
[http://localhost:8080/newview](http://localhost:8080/newview).

![Example of new page](/images/1.0/toolbox/app-drawer-template-newview.png)

## Register the page for the build

When we go to deploy our application to the web, we will use the Polymer CLI
to prepare our files for deployment.  The Polymer CLI will need to know about any
demand-loaded fragments of the application like the lazy-loaded view we just
added.

1.  Open `polymer.json` in a text editor.

1.  Add `src/my-newview.html` to the list of `fragments`.

    The new list should look like this:

    ```
    ...
    "fragments": [
      "src/my-view1.html",
      "src/my-view2.html",
      "src/my-view3.html",
      "src/my-newview.html"
    ]
    ...
    ```

Note: You only need to add files you will async or lazy load into the
`fragments` list.  Any files that are imported using sync `<link rel="import">`
tags should *not* be added to `fragments`.

## Next steps

Now that you've added a new page to your application, learn how to [add 3rd-party
elements to your view](add-elements), or how to [deploy the app to the web](deploy).