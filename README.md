# Dragon Drop
Keyboard accessible drag-and-drop reorder list

## Installation

## npm
```bash
$ npm install drag-on-drop
```

### bower
```bash
$ bower install drag-on-drop
```

## Usage

### `new DragonDrop(container, [options])`

### Browserify/Webpack

```js
import DragonDrop from 'drag-on-drop';

const dragon = new DragonDrop(container, options);
```

### In the browser

```js
const DragonDrop = window.DragonDrop;
const dragon = new DragonDrop(container, options);
```

## API

### `container` (required)
The one and only required parameter is the list HTMLElement that contains the sortable items.

### Options _Object_ (optional)

#### `item` _String_
The selector for the drag items (qualified within container). Defaults to
```js
'li'
```

#### `dragger` _String_
The selector for the keyboard dragger. If set to `false`, the entire item will be used as the dragger. Defaults to 
```ks
'button'
```

#### `activeClass` _String_
The class to be added to the item being dragged. Defaults to 
```js
'dragon-active'
```

#### `inactiveClass` _String_
The class to be added to all of the other items when an item is being dragged. Defaults
```js
'dragon-inactive'
```

#### `announcement` _Object_
The live region announcement configuration object containing the following properties:

##### `grabbed` _Function_
The function called when an item is picked up. The currently grabbed element along with an array of all items are passed as arguments respectively. The function should return a string of text to be announced in the live region. Defaults to
```js
el => `Item ${el.innerText} grabbed`
```

##### `dropped` _Function_
The function called when an item is dropped. The newly dropped item along with an array of all items are passed as arguments respectively. The function should return a string of text to be announced in the live region. Defaults to
```js
el => `Item ${el.innerText} dropped`
```

##### `reorder` _Function_
The function called when the list has been reordered. The newly dropped item along with an array of items are passed as arguments respectively. The function should return a string of text to be announced in the live region. Defaults to
```js
(el, items) => {
  const pos = items.indexOf(el) + 1;
  const text = el.innerText;
  return `The list has been reordered, ${text} is now item ${pos} of ${items.length}`;
}
```

##### `cancel` _Function_
The function called when the reorder is cancelled (via ESC). No arguments passed in. Defaults to
```js
() => 'Reordering cancelled'
```

## Properties
```js
const dragonDrop = new DragonDrop(container);
```
### `dragonDrop.items` _Array_
An array of each of the sortable item element references.

### `dragonDrop.draggers` _Array_
An array of each of the dragger item element references. If instance doesn't have draggers, this will be identical to `dragonDrop.items`.

## Events

### `dragonDrop.on('change', callback)`
The `"change"` event is fired whenever the list is reordered.

## Example

```js
const list = document.getElementById('dragon-list');
const dragonDrop = new DragonDrop(list, {
  item: 'li',
  dragger: '.handle',
  announcement: {
    grabbed: el => `The dragon has grabbed ${el.innerText}`,
    dropped: el => `The dragon has dropped ${el.innerText}`,
    reorder: (el, items) => {
      const pos = items.indexOf(el) + 1;
      const text = el.innerText;
      return `The dragon's list has been reordered, ${text} is now item ${pos} of ${items.length}`;
    },
    cancel: 'The dragon cancelled the reorder'
  }
});

dragonDrop.on('change', () => console.log('list reordered!'));
```
