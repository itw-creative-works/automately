<p align="center">
  <a href="https://itwcreativeworks.com">
    <img src="https://cdn.itwcreativeworks.com/assets/itw-creative-works/images/logo/itw-creative-works-brandmark-black-x.svg" width="100px">
  </a>
</p>

<p align="center">
  <img src="https://img.shields.io/github/package-json/v/itw-creative-works/automately.svg">
  <br>
  <img src="https://img.shields.io/librariesio/release/npm/automately.svg">
  <img src="https://img.shields.io/bundlephobia/min/automately.svg">
  <img src="https://img.shields.io/codeclimate/maintainability-percentage/itw-creative-works/automately.svg">
  <img src="https://img.shields.io/npm/dm/automately.svg">
  <img src="https://img.shields.io/node/v/automately.svg">
  <img src="https://img.shields.io/website/https/itwcreativeworks.com.svg">
  <img src="https://img.shields.io/github/license/itw-creative-works/automately.svg">
  <img src="https://img.shields.io/github/contributors/itw-creative-works/automately.svg">
  <img src="https://img.shields.io/github/last-commit/itw-creative-works/automately.svg">
  <br>
  <br>
  <a href="https://itwcreativeworks.com">Site</a> | <a href="https://www.npmjs.com/package/automately">NPM Module</a> | <a href="https://github.com/itw-creative-works/automately">GitHub Repo</a>
  <br>
  <br>
  <strong>automately</strong> is the official npm module of <a href="https://itwcreativeworks.com">AutoMately</a>, a free module for automating mouse and keyboard inputs on Mac, Windows, and Linux.
</p>

## 🦄 Features
* Automate mouse movements
* Automate mouse clicks
* Automate keyboard inputs

<!-- ### 🔑 Getting an API key
You can use so much of `automately` for free, but if you want to do some advanced stuff, you'll need an API key. You can get one by [signing up for a AutoMately account](https://itwcreativeworks.com/signup). -->

## 📦 Install AutoMately
### Option 1: Install via npm
Install with npm if you plan to use `automately` in a Node project or in the browser.
```shell
npm install automately
```

```js
const { keyboard, Key, mouse, Button, screen, Region } = require('automately');
```

## ⚡️ Using AutoMately
After you have followed the install step, you can start using `automately` to create custom text snippets that you can access on all of your devices

For a more in-depth documentation of this library and the AutoMately service, please visit the official AutoMately website.

### ⌨️ Keyboard Control
#### Configuration
- **autoDelayMs**: Configures the delay between keypresses.<br>
  Example:
  ```js
  keyboard.config.autoDelayMs = 100;
  ```

#### `keyboard.type(...keysOrText);`
Types given keys or strings.

Example:
```js
await keyboard.type(Key.LeftSuper, Key.Space);
await keyboard.type('calculator);
```

#### `keyboard.pressKey(...keys);`
Presses and holds multiple keys.

Example:
```js
await keyboard.pressKey(Key.LeftAlt, Key.F4);
await keyboard.releaseKey(Key.LeftAlt, Key.F4);
```

#### `keyboard.releaseKey(...keys);`
Releases multiple keys.

Example:
```js
await keyboard.pressKey(Key.LeftAlt, Key.F4);
await keyboard.releaseKey(Key.LeftAlt, Key.F4);
```

### 🖱 Mouse Control
#### Configuration
- **autoDelayMs**: Configures the delay between mouse clicks and/or scrolls.<br>
  Example:
  ```js
  mouse.config.autoDelayMs = 100;
  ```
- **mouseSpeed**: Configures mouse movement speed in pixels per second.<br>
  Example:
  ```js
  mouse.config.mouseSpeed = 1000;
  ```

#### `mouse.setPosition(point);`
Moves the mouse cursor to a given position instantly.

Example:
```js
await mouse.setPosition(new Point(500, 500));
```

#### `mouse.getPosition();`
Returns a Promise resolving to the current cursor position.

Example:
```js
const position = await mouse.getPosition();
console.log(position); // Point { x: 500, y: 500 }
```

#### `mouse.move(path, movementFunction);`
Moves the mouse cursor along a given path.

Example:
```js
await mouse.move(straightTo(centerOf(await screen.find('image.png'))));
```

### 🖥 Screen Control
#### Configuration
- **confidence**: Specifies the required matching percentage for image searching.<br>
  Example:
  ```js
  screen.config.confidence = 0.95;
  ```
- **autoHighlight**: Enables automated highlighting of image search results.<br>
  Example:
  ```js
  screen.config.autoHighlight = true;
  ```
- **highlightDurationMs**: Configures the duration of highlight window display.<br>
  Example:
  ```js
  screen.config.highlightDurationMs = 500;
  ```
- **highlightOpacity**: Configures the opacity of highlight windows.<br>
  Example:
  ```js
  screen.config.highlightOpacity = 0.7;
  ```
- **resourceDirectory**: Configures the asset directory for image resources.<br>
  Example:
  ```js
  screen.config.resourceDirectory = '/path/to/resources';
  ```

#### `screen.capture(filePath);`
Captures a screenshot and stores it to the filesystem.

Example:
```js
await screen.capture('/path/to/screenshot.png');
```

#### `screen.captureRegion(region, filePath);`
Captures a screenshot of a specific region and stores it to the filesystem.

Example:
```js
const region = new Region(0, 0, 100, 100);
await screen.captureRegion(region, '/path/to/region_screenshot.png');
```

#### `screen.find(imageResource);`
Finds a match for a given image on the screen.

Example:
```js
await mouse.move(straightTo(centerOf(await screen.find('image.png'))));
```

#### `screen.findAll(imageResource);`
Finds all matches for a given image on the screen.

Example:
```js
const matches = await screen.findAll('image.png');
for (const match of matches) {
    await mouse.move(straightTo(centerOf(match)));
}
```

#### `screen.highlight(region);`
Displays an opaque window overlay for easier visual follow-up.

Example:
```js
await screen.highlight(await screen.find('image.png'));
```

#### `screen.on(query, callback);`
Registers callbacks executed upon finding a match for a template image.

Example:
```js
const colorQuery = pixelWithColor(new RGBA(0, 0, 0, 255));
const secondQuery = pixelWithColor(new RGBA(43, 45, 48, 255));

screen.on(colorQuery, async (matchResult) => {
    await mouse.move(straightTo(matchResult.location));
});

screen.on(secondQuery, async (matchResult) => {
    console.log('Second query found');
});

await screen.find(colorQuery); // Triggers callback
await mouse.move(straightTo(new Point(100, 100)));
await screen.find(secondQuery); // Triggers callback
await screen.find(colorQuery); // Triggers callback again
```

#### `screen.waitFor(imageResource, timeoutMs, intervalMs);`
Waits for a match for a given image on the screen within a specified timeout.

Example:
```js
await mouse.move(straightTo(centerOf(await screen.waitFor('image.png', 3000, 500))));
```

#### `screen.colorAt(point);`
Returns RGBA color information at a specified pixel location.

Example:
```js
const color = await screen.colorAt(new Point(0, 0));
console.log(color); // RGBA { R: 0, G: 0, B: 0, A: 255 }
```

#### `screen.width();`
Returns the main screen's width in pixels.

Example:
```js
const width = await screen.width();
console.log(width); // e.g. 1920
```

#### `screen.height();`
Returns the main screen's height in pixels.

Example:
```js
const height = await screen.height();
console.log(height); // e.g. 1080
```

## 📝 What Can AutoMately do?
AutoMately is a free text snippet manager that lets you [create custom text snippets](https://itwcreativeworks.com) that you can access on all of your devices!

## 🗨️ Final Words
If you are still having difficulty, we would love for you to post
a question to [the AutoMately issues page](https://github.com/itw-creative-works/automately/issues). It is much easier to answer questions that include your code and relevant files! So if you can provide them, we'd be extremely grateful (and more likely to help you find the answer!)

## 📚 Projects Using this Library
* [ITW Creative Works](https://itwcreativeworks.com)
* [Somiibo](https://somiibo.com)
* [Slapform](https://slapform.com)
* [StudyMonkey](https://studymonkey.ai)
* [DashQR](https://dashqr.com)
* [Replyify](https://replyify.app)
* [SoundGrail](https://soundgrail.com)
* [Trusteroo](https://trusteroo.com)

Ask us to have your project listed! :)
