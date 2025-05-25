◀️ [Home](../README.md)

# HTML

- Label:
```html
<button>
```
- Element:
```html
<button type="button" data-action="submit-form">Submit</button>
```

## Buttons

`<button>`: Used to create clickable buttons that trigger actions, typically in forms or with JavaScript. The `data-*` attributes, such as `data-action`, are **custom attributes** often used to store extra information about an element, particularly for JavaScript functionality.

```html
<button type="button" data-action="submit-form">Submit</button>
```
`data-action`: Custom HTML attribute without built-in functionality, storing button-specific data for JavaScript use.Defines button actions (e.g., "submit-form") and enables cleaner JavaScript implementation.

```js
// Add click event listeners to AJAX-triggered ETL buttons
document.querySelectorAll(".etl-trigger").forEach((button) => {
    button.addEventListener("click", () => {
        const action = button.getAttribute("data-action");
        console.log(`ETL button clicked: ${action}`);

        // Make an AJAX POST request for ETL actions
        ...
```

## Forms

`<form>`: Used to create a form for uploading files, and the `enctype="multipart/form-data"` attribute specifies how the form data should be encoded when it is sent to the server.

```html
<form id="upload-form" enctype="multipart/form-data" method="POST" action="/upload">
  <input type="file" name="file" />
  <button type="submit">Upload</button>
</form>
```

1. id="upload-form":
- Unique form identifier for JavaScript/CSS references

2. enctype="multipart/form-data":
- Required for file uploads
- Enables proper binary data encoding

3. method="POST":
- Sends form data via POST method
- Preferred for large file uploads

4. action="/upload":
- Specifies server endpoint for form submission

## iFrame

`<iframe>` (Inline Frame) is used to embed another HTML document within the current document. It allows you to display content from another web page inside your page, like a video, map, or other web content.

```html
<iframe src="URL" width="width" height="height"></iframe>
```

## Lists

`<ul>`: Stands for **unordered list** and acts as the container for the list items.

`<li>`: Stands for **list item** and represents each item in the list.

```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ul>
```

## Navigation

`<nav>`: represents a **section of the document intended for navigation links**. It is used to group links that allow users to navigate to different sections of the same page, other pages of the website, or external sites.

```html
<nav>
  <ul>
    <li><a href="index.html">Home</a></li>
    <li><a href="services.html">Services</a></li>
    <li><a href="/portfolio">Portfolio</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>
```

## URLs

`<a>`: Stands for “anchor.” Used to create **hyperlinks**, which are links to other web pages, files, locations within the same page, email addresses, or other resources.

```html
<a href="URL">Link Text</a>
```