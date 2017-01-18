# [rkgttr-elements](https://github.com/rkgttr/rkgttr-elements)

[![NPM version](http://img.shields.io/npm/v/rkgttr-elements.svg?style=flat-square)](https://www.npmjs.com/package/rkgttr-elements)
[![NPM downloads](http://img.shields.io/npm/dm/rkgttr-elements.svg?style=flat-square)](https://www.npmjs.com/package/rkgttr-elements)
[![Build Status](http://img.shields.io/travis/rkgttr/rkgttr-elements/master.svg?style=flat-square)](https://travis-ci.org/rkgttr/rkgttr-elements)
[![Coverage Status](https://img.shields.io/coveralls/rkgttr/rkgttr-elements.svg?style=flat-square)](https://coveralls.io/rkgttr/rkgttr-elements)
[![Dependency Status](http://img.shields.io/david/rkgttr/rkgttr-elements.svg?style=flat-square)](https://david-dm.org/rkgttr/rkgttr-elements)

> HTML Components helpers, shamelessly inspired by https://github.com/davidgilbertson/know-it-all.

### How to Install

```sh
$ npm install rkgttr-elements --save-dev
```
or

```sh
$ yarn add rkgttr-elements --dev
```

### Getting Started

Build a component this way:

```js
import Publisher from 'rkgttr-publisher';
import uuid from 'rkgttr-uuid';
import { div, img, h2, p, a } from 'Elements';

const MediaObject = (initialData) => {
  let el = null,
    uid = uuid();

  const render = data => (
    div({
        className: 'media',
        dataset: {uid: uid}
      },
      a({
          className: 'img',
          href: '#',
          onclick: (e) => {
            e.preventDefault();
            Publisher.trigger(`data:change`, {uid:uid, title: 'Hello', name: 'Mars' });
          }
        },
        img({ src: 'http://placehold.it/350x150', alt: 'Alt text' })
      ),
      div({ className: 'bd' },
        h2(data.title),
        p(data.name)
      )
    )
  );

  const update = (prevEl, newData) => {
    if(newData.uid !== uid) {
      return prevEl;
    }
    const nextEl = render(newData);

    if (nextEl.isEqualNode(prevEl)) {
      console.warn(`render() was called but there was no change in the rendered output`, el);
    } else {
      prevEl.parentElement.replaceChild(nextEl, prevEl);
    }

    return nextEl;
  };

  Publisher.on(`data:change`, (newData) => el = update(el, newData));

  el = render(initialData);

  return el;

};
document.body.appendChild(MediaObject({ title: 'Hello', name: 'World' }));
document.body.appendChild(MediaObject({ title: 'Hi', name: 'World' }));
```



### License

MIT © 2016 Erik Guittiere
