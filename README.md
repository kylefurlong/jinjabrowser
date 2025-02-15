# 🌊 Mirage

A powerful, lightweight templating engine for the browser with Jinja-like syntax, smart caching, and async loading support.

## Features

- 🎯 Full template inheritance with `extends` and `block`
- 🔄 Powerful control structures (`if/else`, `for` loops)
- 🛠 Macros for reusable template components
- 🔍 Rich set of built-in filters
- 📦 Smart template loading and caching
- 💾 LocalStorage caching with automatic invalidation
- ⚡ Async loading from external sources
- 🔌 Custom filter support
- 🐛 Detailed error reporting

## Installation

```html
<script src="mirage.min.js"></script>
```

## Quick Start

Define your templates using script tags:

```html
<!-- Inline template -->
<script type="text/template+mirage" id="greeting">
  <h1>Hello, {{ name|title }}!</h1>
  {% if messages %}
    <ul>
    {% for msg in messages %}
      <li>{{ loop.index }}. {{ msg }}</li>
    {% endfor %}
    </ul>
  {% endif %}
</script>

<!-- External template -->
<script type="text/template+mirage" id="header" src="/templates/header.html"></script>
```

Render the template:

```javascript
// Templates are loaded automatically on DOMContentLoaded
const result = mirage.template('greeting', {
  name: 'john doe',
  messages: ['Welcome!', 'How are you?']
});
```

## Template Inheritance

Define a base template:

```html
<script type="text/template+mirage" id="base">
<!DOCTYPE html>
<html>
  <head>
    <title>{% block title %}Default Title{% endblock %}</title>
  </head>
  <body>
    <header>{% block header %}{% endblock %}</header>
    <main>{% block content %}{% endblock %}</main>
    <footer>{% block footer %}Default Footer{% endblock %}</footer>
  </body>
</html>
</script>
```

Extend it in child templates:

```html
<script type="text/template+mirage" id="page">
{% extends 'base' %}

{% block title %}My Custom Page{% endblock %}

{% block content %}
  <h1>Welcome!</h1>
  <p>This is my custom content.</p>
{% endblock %}
</script>
```

## Macros

Define reusable template components:

```html
<script type="text/template+mirage" id="forms">
{% macro input(name, value='', type='text', label='') %}
  <div class="form-group">
    {% if label %}
      <label for="{{ name }}">{{ label }}</label>
    {% endif %}
    <input type="{{ type }}" 
           name="{{ name }}" 
           value="{{ value }}" 
           id="{{ name }}">
  </div>
{% endmacro %}

{% macro select(name, options) %}
  <select name="{{ name }}">
    {% for option in options %}
      <option value="{{ option.value }}">
        {{ option.label }}
      </option>
    {% endfor %}
  </select>
{% endmacro %}
</script>
```

## Smart Template Loading

Mirage supports loading templates from external files with intelligent caching:

```html
<script type="text/template+mirage" 
        id="layout" 
        src="/templates/layout.html">
</script>
```

Features:
- Automatic LocalStorage caching
- ETag and Last-Modified support for cache validation
- Graceful fallback to cached content when offline
- Automatic cache invalidation
- Concurrent loading of multiple templates

Cache Management:
```javascript
// Clear specific template cache
await mirage.clearCache('layout');

// Clear all template caches
await mirage.clearCache();
```

## Built-in Filters

- `capitalize`: Capitalizes first letter
- `upper`: Converts to uppercase
- `lower`: Converts to lowercase
- `title`: Capitalizes words
- `trim`: Removes whitespace
- `length`: Returns length of string/array
- `default(value)`: Sets default value
- `join(separator)`: Joins array elements
- `reverse`: Reverses string/array
- `first`: Gets first element
- `last`: Gets last element
- `sort`: Sorts array
- `round(precision)`: Rounds number
- `truncate(length, ending)`: Truncates text
- `safe`: Marks content as safe HTML
- `escape`: HTML escapes content

## Loop Context

```html
{% for item in items %}
  {{ loop.index }}      {# 0-based index #}
  {{ loop.index1 }}     {# 1-based index #}
  {{ loop.revindex }}   {# Reverse index #}
  {{ loop.revindex1 }}  {# Reverse 1-based index #}
  {{ loop.first }}      {# True if first iteration #}
  {{ loop.last }}       {# True if last iteration #}
  {{ loop.length }}     {# Total number of items #}
  {{ loop.cycle('odd', 'even') }} {# Cycle values #}
{% endfor %}
```

## API Reference

### Core Methods

- `mirage.init()`: Initialize the library (returns Promise)
- `mirage.template(name, vars)`: Render a template
- `mirage.addTemplate(id, templateString)`: Add template programmatically
- `mirage.loadTemplate(url, id)`: Load template asynchronously
- `mirage.addFilter(name, fn)`: Add custom filter
- `mirage.getTemplateNames()`: List available templates
- `mirage.clearCache()`: Clear template cache

## Performance Considerations

1. Template Caching:
   - Compiled templates are cached in memory
   - Raw templates are cached in LocalStorage
   - Smart cache invalidation using ETags and Last-Modified headers

2. Loading Optimizations:
   - Concurrent loading of multiple templates
   - Fallback to cached content when offline
   - Lazy compilation of templates

3. Rendering Performance:
   - Single-pass compilation
   - Optimized string concatenation
   - Efficient variable lookup

## Browser Support

Works in all modern browsers (Chrome, Firefox, Safari, Edge).

## License

This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or distribute this software, either in source code form or as a compiled binary, for any purpose, commercial or non-commercial, and by any means.

In jurisdictions that recognize copyright laws, the author or authors of this software dedicate any and all copyright interest in the software to the public domain. We make this dedication for the benefit of the public at large and to the detriment of our heirs and successors. We intend this dedication to be an overt act of relinquishment in perpetuity of all present and future rights to this software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <https://unlicense.org/>

## Contributing

Contributions are welcome! Feel free to open issues and pull requests.
