# Theme support in web and/or react application
Having multiple themes in your application can lure many users as some people like light theme whereas some people choose to apply dark theme.

There are different ways to enable theme support to your web and/or react application. But the most efficient and easy way to achieve this is by toggling `data-theme` attribute of your root `html`. In this article we will learn about the same.

## Pre-requisite
* Good knowledge of `HTML` and `CSS`
* Basic knowledge of `JavaScript`

## Preview
![https://raw.githubusercontent.com/rajyadavnp/theme-web/main/demo.gif](https://raw.githubusercontent.com/rajyadavnp/theme-web/main/demo.gif)

##  Setup UI (HTML)
Add `data-theme` attribute to the `<html>` tag to set the theme by `default` (_not mandatory, uses properties from `:root`, if not specified_)
```html
<html data-theme="light-green">
    <head>
        <title>My website</title>
        <link rel="stylesheet" href="styles.css" />
    </head>
    <body>
        <div>
            <div>
                <h2>This is some text to preview.</h2>
                <button>Action Button</button>
            </div>
            <label for="light-green">
                <input id="light-green" value="" type="radio" name="theme" onchange="toggleTheme(this.id)"/>
                Light (Green) theme
            </label>

            <label for="light-blue">
                <input id="light-blue" value="" type="radio" name="theme" onchange="toggleTheme(this.id)"/>
                Light (Blue) theme
            </label>

            <label for="dark-green">
                <input id="dark-green" type="radio" name="theme"
                    onchange="toggleTheme(this.id)"/>
                Dark (Green) theme
            </label>

            <label for="dark-blue">
                <input id="dark-blue" type="radio" name="theme" onchange="toggleTheme(this.id)"/>
                Dark (Blue) theme
            </label>

        </div>
    </body>
</html>
```
In react, replace  `for` with `htmlFor` and `onchange` event with `onChange={(event) => toggleTheme(event.target.id)}`
```html
<label htmlFor="light-green">
    <input id="light-green" type="radio" name="theme" onChange={(event) => toggleTheme(event.target.id)}/>
    Light (Green) theme
</label>

```

## Style the UI (CSS)
Define variables for all the themes and use `var(--variable)` throughout the app styles.
```css
:root { /* Default theme */
    --bg: #ffffff;
    --text: #000000;
    --accent: seagreen;
}

[data-theme='light-green'] {
    --bg: #ffffff;
    --text: #000000;
    --accent: seagreen;
}

[data-theme='light-blue'] {
    --bg: #ffffff;
    --text: #000000;
    --accent: royalblue;
}

[data-theme='dark-green'] {
    --bg: #252545;
    --text: #ffffff;
    --accent: seagreen;
}

[data-theme='dark-blue'] {
    --bg: #252545;
    --text: #ffffff;
    --accent: royalblue;
}

html, body {
    margin: 0;
    background: var(--bg);
    color: var(--text);
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
}

button {
    display: block;
    background: var(--accent);
    color: white;
    border: none;
    padding: 8px 12px;
    cursor: pointer;
    margin: 20px 0;
}

label {
    display: block;
    padding: 10px 0;
}
```

## Toggle theme (JavaScript)
Now, we can set `data-theme` attribute of `<html>` tag. `document.documentElement` returns the root `<html>` element
```javascript
const toggleTheme = (themeData) => {
    document.documentElement.setAttribute("data-theme", themeData);
}
```

## Remembering the preferences
To remember the last setting by the user, we can use `localStorage` and store last selected theme.
```javascript
const toggleTheme = (themeData) => {
    document.documentElement.setAttribute("data-theme", themeData);
    // Add this line
    localStorage.setItem("data-theme", themeData);
}
```
Read from the `localStorage` and apply the last theme as soon as the app contents are ready.
```javascript
const setLastTheme = () => {
    let themeData = localStorage.getItem("data-theme");
    document.documentElement.setAttribute("data-theme", themeData);
}

document.addEventListener("DOMContentLoaded", function() {
    setLastTheme(); 
});
```

Check the `radio` button with last selected theme.
```javascript
const setCurrentThemeCheck = () => {
    let ele = document.getElementsByName("theme");
    let currentTheme = localStorage.getItem("data-theme") || "light-green";

    ele.forEach(e => {
        if (e.id === currentTheme) e.checked = true;
    })
}

setCurrentThemeCheck();

```
In react, use `useEffect` hook to apply last selected theme and check the  `radio` button.
```javascript
useEffect(() => {
    setLastTheme();
    setCurrentThemeCheck();
}, []);
```

#### And ta-da, your application is now ready with multiple themes.

View an example code or raise any issues, [here](https://github.com/rajyadavnp/theme-web)