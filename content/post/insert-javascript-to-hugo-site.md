---
title: "JavaScript in Hugo"
date: 2024-10-08
---

Running JavaScript inside a Hugo site is quite straightforward since Hugo generates static HTML files that you can enhance with client-side JavaScript. You can include JavaScript for a variety of purposes, such as adding dynamic features, handling forms, or integrating third-party services.

Here are the steps to add and run JavaScript in your Hugo site:

### 1. **Add JavaScript Files to Your Hugo Site**

    - Place your JavaScript files inside the `static/js/` folder of your Hugo site. Files in the `static` directory are served as-is, meaning they will be available in the root of your site when deployed.
    - For example:
        - Create the folder `static/js/` in your Hugo project.
        - Add your custom JavaScript file, e.g., `main.js`, to this folder: `static/js/main.js`.

### 2. **Include JavaScript in Your Layout**

   You need to link the JavaScript file in your HTML templates, typically in the `<head>` or before the closing `</body>` tag of your site's layout files.

   For example, in your `layouts/_default/baseof.html` (or whichever layout file you use), add the following line before the closing `</body>` tag:

   ```html
   <script src="/js/main.js"></script>
   ```

   - This includes the `main.js` file located in `static/js/` into your HTML.
   - If you have a theme, place this in the appropriate layout file in the theme's `layouts/` directory (usually in the base template, e.g., `baseof.html`).

### 3. **Inline JavaScript (Optional)**

   You can also write inline JavaScript directly in your template files if you have small scripts that don't need to be placed in a separate file. You can do this within a `<script>` tag.

   For example, inside your `layouts/_default/single.html` or other templates:

   ```html
   <script>
     console.log('Hello, Hugo! - Inline JavaScript');
   </script>
   ```

### 4. **Using Hugo's Asset Bundling (Optional)**

    If you want to bundle, minify, or concatenate your JavaScript files, Hugo has built-in asset pipeline features.

    You can create a file like `assets/js/main.js` and process it using Hugo's asset processing features. For example, in your template:

    ```go
    {{ $js := resources.Get "js/main.js" | minify | fingerprint }}
    <script src="{{ $js.Permalink }}" integrity="{{ $js.Data.Integrity }}"></script>
    ```
    - This uses Hugo's asset pipeline to process, minify, and add a fingerprint for caching purposes.

### 5. **Running JavaScript in Your Hugo Site**

Once your JavaScript file is linked, you can add whatever functionality you need. For example, if you add the following `main.js` to your `static/js/main.js` file:

```javascript
// main.js
document.addEventListener('DOMContentLoaded', function() {
    console.log("Hugo site is ready!");
    // Add more JS functionality here
});
```

This script will run when the DOM is fully loaded, and you'll see the output in the browser's developer console.

### 6. **Using External Libraries**

If you want to use external JavaScript libraries (e.g., jQuery, Bootstrap), you can include them either by:
    - Linking to a CDN:
        ```html
        <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
        ```
    - Downloading the libraries and placing them in the `static` folder, just like your custom JS files.

### Example Setup

**Project Structure:**

```
my-hugo-site/
├── assets/
├── content/
├── layouts/
│   └── _default/
│       └── baseof.html
├── static/
│   └── js/
│       └── main.js
└── config.toml
```

**baseof.html:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .Title }}</title>
</head>
<body>
    {{ block "main" . }}{{ end }}
    <script src="/js/main.js"></script>
</body>
</html>
```

**main.js (in static/js/):**

```javascript
document.addEventListener('DOMContentLoaded', function() {
    alert('Hello from Hugo!');
});
```

### 7. **Using JavaScript with Shortcodes (Optional)**

Hugo supports shortcodes, and you can use shortcodes to embed JavaScript into specific content. For example, you could create a shortcode in `layouts/shortcodes/my-js.html` that injects JavaScript into a post:

```html
<script>
  console.log('This JavaScript is inserted via a shortcode');
</script>
```

Then use the shortcode in your content file:

    ```markdown
    {{< my-js >}}
    ```

### Conclusion

To run JavaScript in your Hugo site:
1. Place the JavaScript files in the `static/` folder.
2. Include them in your layout files using `<script>` tags.
3. Optionally use Hugo’s asset pipeline to manage, minify, and fingerprint your JavaScript.

This setup should allow you to easily add dynamic client-side functionality to your Hugo-generated static site.