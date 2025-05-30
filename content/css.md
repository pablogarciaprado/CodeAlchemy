◀️ [Home](../README.md)

# CSS

## Font Awesome
Popular icon library

The following link tag imports the Font Awesome library (version 4.7.0) to the HTML document, enabling the use of a wide variety of icons throughout the webpage.
```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
```

```html
<i class="fa fa-sign-out" aria-hidden="true"></i> <!-- Sign Out icon -->
<i class="fa fa-heart"></i>  <!-- Heart icon -->
<i class="fa fa-camera"></i> <!-- Camera icon -->
```

## z-index
The z-index CSS property controls the stacking order of elements in the third dimension (the z-axis). Think of it as layers stacked on top of each other, like sheets of paper on a desk.

Here's a visual explanation of how z-index works in a table:
```
Higher z-index                  Lower z-index
     ↑                              ↓
    
z-index: 10  → Buttons container (always on top)
z-index: 4   → Frozen header cells
z-index: 3   → Frozen column cells
z-index: 2   → Regular header cells
z-index: 1   → Regular table cells
```

In a table, we need z-index because:
- Frozen columns use position: sticky to stay in place while scrolling
- Multiple elements are positioned on top of each other
- We need to control which elements appear above others

For example:
- Buttons should always be visible (highest z-index)
- Frozen headers should appear above frozen columns
- Frozen columns should appear above regular cells
- Regular headers should appear above regular cells
