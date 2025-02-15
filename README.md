# JinjaBrowser

A lightweight yet powerful Jinja-like templating engine for the browser. JinjaBrowser brings the flexibility of server-side Jinja templates to client-side JavaScript, supporting inheritance, macros, filters, and more.

## Features

- 🎯 Full template inheritance with `extends` and `block`
- 🔄 Powerful control structures (`if/else`, `for` loops)
- 🛠 Macros for reusable template components
- 🔍 Rich set of built-in filters
- 🔌 Custom filter support
- 📦 Template includes
- 🔄 Asynchronous template loading
- ⚡ Fast compilation and rendering
- 🐛 Detailed error reporting

## Installation

```html
<script src="jinja-browser.min.js"></script>
```

## Quick Start

1. Define your templates using script tags:

```html
<script type="text/template+jinja" id="greeting">
  <h1>Hello, {{ name|title }}!</h1>
  {% if messages %}
    <ul>
    {% for msg in messages %}
      <li>{{ loop.index }}. {{ msg }}</li>
    {% endfor %}
    </ul>
  {% endif %}
</script>
```

2. Render the template:

```javascript
const result = jinja.template('greeting', {
  name: 'john doe',
  messages: ['Welcome!', 'How are you?']
});
```

## Template Inheritance

Define a base template:

```html
<script type="text/template+jinja" id="base">
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
<script type="text/template+jinja" id="page">
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
<script type="text/template+jinja" id="forms">
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

Use macros in other templates:

```html
<script type="text/template+jinja" id="signup-form">
{% include 'forms' %}

<form>
  {{ input('username', '', 'text', 'Username') }}
  {{ input('password', '', 'password', 'Password') }}
  {{ select('country', countries) }}
</form>
</script>
```

## Built-in Filters

JinjaBrowser includes many useful filters:

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

Rich context variables in for loops:

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

## Async Template Loading

Load templates dynamically:

```javascript
await jinja.loadTemplate('/templates/header.html', 'header');
const result = jinja.template('header', data);
```

## Custom Filters

Add your own filters:

```javascript
jinja.addFilter('multiply', (val, factor) => val * factor);

// Use in template
{{ price|multiply:1.2|round:2 }}
```

## Error Handling

Detailed error reporting:

```javascript
try {
  const result = jinja.template('myTemplate', data);
} catch (e) {
  if (e.name === 'TemplateError') {
    console.error(`Template error at line ${e.line}: ${e.message}`);
  }
}
```

## API Reference

### Core Methods

- `jinja.init()`: Initialize the library
- `jinja.template(name, vars)`: Render a template
- `jinja.addTemplate(id, templateString)`: Add template programmatically
- `jinja.loadTemplate(url, id)`: Load template asynchronously
- `jinja.addFilter(name, fn)`: Add custom filter
- `jinja.getTemplateNames()`: List available templates
- `jinja.clearCache()`: Clear template cache

## Browser Support

Works in all modern browsers (Chrome, Firefox, Safari, Edge).

## License

MIT

## Contributing

Contributions welcome! Please read the contributing guidelines before submitting PRs.
