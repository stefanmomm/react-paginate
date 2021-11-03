# react-paginate

[![NPM](https://nodei.co/npm/react-paginate.png?downloads=true)](https://nodei.co/npm/react-paginate/)
[![Build Status](https://travis-ci.org/AdeleD/react-paginate.svg?branch=master)](https://travis-ci.org/AdeleD/react-paginate)

**A ReactJS component to render a pagination.**

By installing this component and writing only a little bit of CSS you can obtain this:
Note: You should write your own css to obtain this UI. This package do not provide any css.

<img src="https://cloud.githubusercontent.com/assets/2084833/24840237/7accb75a-1d1e-11e7-9abb-818431398b91.png" alt="Pagination demo 2" />

or

<img src="https://cloud.githubusercontent.com/assets/2084833/24840230/594e4ea4-1d1e-11e7-8b34-bde943b4793d.png" alt="Pagination demo 1" />

## Installation

Install `react-paginate` with [npm](https://www.npmjs.com/):

```
npm install react-paginate --save
```

## Usage

```javascript
import React, { useEffect, useState } from 'react';
import ReactDOM from 'react-dom';
import ReactPaginate from 'react-paginate';

// Example items, to simulate fetching from another resources.
const items = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14];

function Items({ currentItems }) {
  return (
    <>
      {currentItems &&
        currentItems.map((item) => (
          <div>
            <h3>Item #{item}</h3>
          </div>
        ))}
    </>
  );
}

function PaginatedItems({ itemsPerPage }) {
  // We start with an empty list of items.
  const [currentItems, setCurrentItems] = useState(null);
  const [pageCount, setPageCount] = useState(0);
  // Here we use item offsets; we could also use page offsets
  // following the API or data you're working with.
  const [itemOffset, setItemOffset] = useState(0);

  useEffect(() => {
    // Fetch items from another resources.
    const endOffset = itemOffset + itemsPerPage;
    console.log(`Loading items from ${itemOffset} to ${endOffset}`);
    setCurrentItems(items.slice(itemOffset, endOffset));
    setPageCount(Math.ceil(items.length / itemsPerPage));
  }, [itemOffset, itemsPerPage]);

  // Invoke when user click to request another page.
  const handlePageClick = (event) => {
    const newOffset = (event.selected * itemsPerPage) % items.length;
    console.log(
      `User requested page number ${event.selected}, which is offset ${newOffset}`
    );
    setItemOffset(newOffset);
  };

  return (
    <>
      <Items currentItems={currentItems} />
      <ReactPaginate
        breakLabel="..."
        nextLabel="next >"
        onPageChange={handlePageClick}
        pageRangeDisplayed={5}
        pageCount={pageCount}
        previousLabel="< previous"
        renderOnZeroPageCount={null}
      />
    </>
  );
}

// Add a <div id="container"> to your HTML to see the componend rendered.
ReactDOM.render(
  <PaginatedItems itemsPerPage={4} />,
  document.getElementById('container')
);
```

Test it on [CodePen](https://codepen.io/monsieurv/pen/abyJQWQ).

You can also read the code of [demo/js/demo.js][1] to quickly understand how to make `react-paginate` work with a list of objects.

Finally there is this **[CodePen demo](https://codepen.io/monsieurv/pen/yLoMxYQ)**, witch features fetching sample code (using GitHub API) and two synchronized pagination widgets.

## Props

| Name                     | Type       | Description                                                                                                                                                            |
| ------------------------ | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `pageCount`              | `Number`   | **Required.** The total number of pages.                                                                                                                               |
| `pageRangeDisplayed`     | `Number`   | The range of pages displayed.                                                                                                                                          |
| `marginPagesDisplayed`   | `Number`   | The number of pages to display for margins.                                                                                                                            |
| `previousLabel`          | `Node`     | Label for the `previous` button.                                                                                                                                       |
| `nextLabel`              | `Node`     | Label for the `next` button.                                                                                                                                           |
| `breakLabel`             | `Node`     | Label for ellipsis.                                                                                                                                                    |
| `breakClassName`         | `String`   | The classname on tag `li` of the ellipsis element.                                                                                                                     |
| `breakLinkClassName`     | `String`   | The classname on tag `a` of the ellipsis element.                                                                                                                      |
| `onPageChange`           | `Function` | The method to call when a page is clicked. Exposes the current page object as an argument.                                                                             |
| `onPageActive`           | `Function` | The method to call when an active page is clicked. Exposes the active page object as an argument.                                                                      |
| `initialPage`            | `Number`   | The initial page selected, in [uncontrolled mode](https://reactjs.org/docs/uncontrolled-components.html). Do not use with `forcePage` at the same time.                |
| `forcePage`              | `Number`   | To override selected page with parent prop. Use this if you want to [control](https://reactjs.org/docs/forms.html#controlled-components) the page from your app state. |
| `disableInitialCallback` | `boolean`  | Disable `onPageChange` callback with initial page. Default: `false`                                                                                                    |
| `containerClassName`     | `String`   | The classname of the pagination container.                                                                                                                             |
| `className`              | `String`   | Same as `containerClassName`. For use with [styled-components](https://styled-components.com/) & other CSS-in-JS.                                                      |
| `pageClassName`          | `String`   | The classname on tag `li` of each page element.                                                                                                                        |
| `pageLinkClassName`      | `String`   | The classname on tag `a` of each page element.                                                                                                                         |
| `pageLabelBuilder`       | `Function` | Function to set the text on page links. Defaults to `(page) => page`                                                                                                   |
| `activeClassName`        | `String`   | The classname for the active page.                                                                                                                                     |
| `activeLinkClassName`    | `String`   | The classname on the active tag `a`.                                                                                                                                   |
| `previousClassName`      | `String`   | The classname on tag `li` of the `previous` button.                                                                                                                    |
| `nextClassName`          | `String`   | The classname on tag `li` of the `next` button.                                                                                                                        |
| `previousLinkClassName`  | `String`   | The classname on tag `a` of the `previous` button.                                                                                                                     |
| `nextLinkClassName`      | `String`   | The classname on tag `a` of the `next` button.                                                                                                                         |
| `disabledClassName`      | `String`   | The classname for disabled `previous` and `next` buttons.                                                                                                              |
| `disabledLinkClassName`  | `String`   | The classname on tag `a` for disabled `previous` and `next` buttons.                                                                                                   |
| `hrefBuilder`            | `Function` | The method is called to generate the `href` attribute value on tag `a` of each page element.                                                                           |
| `hrefAllControls`        | `Bool`     | By default the `hrefBuilder` add `href` only to active controls. Set this prop to `true` so `href` are generated on all controls ([see](https://github.com/AdeleD/react-paginate/issues/242)).                                                                           |
| `extraAriaContext`       | `String`   | DEPRECATED: Extra context to add to the `aria-label` HTML attribute.                                                                                                   |
| `ariaLabelBuilder`       | `Function` | The method is called to generate the `aria-label` attribute value on each page link                                                                                    |
| `eventListener`          | `String`   | The event to listen onto before changing the selected page. Default is: `onClick`.                                                                                     |
| `renderOnZeroPageCount`  | `Function` | A render fonction called when `pageCount` is zero. Let the Previous / Next buttons displayed by default (`undefined`). Display nothing when `null` is provided.        |
| `prevRel`                | `String`   | The `rel` property on the `a` tag for the prev page control. Default value `prev`. Set to `null` to disable.                                                           |
| `nextRel`                | `String`   | The `rel` propery on the `a` tag for the next page control. Default value `next`. Set to `null` to disable.                                                            |
| `prevPageRel`            | `String`   | The `rel` property on the `a` tag just before the selected page. Default value `prev`. Set to `null` to disable.                                                       |
| `selectedPageRel`        | `String`   | The `rel` propery on the `a` tag for the selected page. Default value `canonical`. Set to `null` to disable.                                                           |
| `nextPageRel`            | `String`   | The `rel` property on the `a` tag just after the selected page. Default value `next`. Set to `null` to disable.                                                        |
| `showFirstLabel`         | `Bool`     | Optional First Page button. Default value `false`. Set to `true` to enable.                                                                                            |
| `firstLabel`             | `Node`     | Label for the First button.                                                                                                                                            |
| `firstClassName`         | `String`   | The classname on tag `li` of the `first` button.                                                                                                                       |
| `firstLinkClassName`     | `String`   | The classname on tag `a` of the `first` button.                                                                                                                        |
| `firstAriaLabel`         | `String`   | `aria-label` for the first button. Default: `First page`.                                                                                                              |
| `firstRel`               | `String`   | The `rel` property on the `a` tag for the first page control. Default value `first`. Set to `null` to disable.                                                         |
| `showLastLabel`          | `Bool`     | Optional Last Page button. Default value `false`. Set to `true` to enable.                                                                                             |
| `lastLabel`              | `Node`     | Label for the Last button.                                                                                                                                             |
| `lastClassName`          | `String`   | The classname on tag `li` of the `last` button.                                                                                                                        |
| `lastLinkClassName`      | `String`   | The classname on tag `a` of the `last` button.                                                                                                                         |
| `lastAriaLabel`          | `String`   | `aria-label` for the first button. Default: `Last page`.                                                                                                               |
| `lastRel`                | `String`   | The `rel` property on the `a` tag for the first page control. Default value `last`. Set to `null` to disable.                                                          |

## Demo

To run the demo locally, clone the repository and move into it:

```console
git clone git@github.com:AdeleD/react-paginate.git
cd react-paginate
```

Install dependencies:

```console
make install
```

Prepare the demo:

```console
make demo
```

Run the server:

```console
make serve
```

Open your browser and go to [http://localhost:3000/](http://localhost:3000/)

<img src="https://cloud.githubusercontent.com/assets/2084833/24840241/7c95b7b2-1d1e-11e7-97e3-83b9c7a1f832.gif" alt="Pagination demo" />

## Contribute

1. [Submit an issue](https://github.com/AdeleD/react-paginate/issues)
2. Fork the repository
3. Create a dedicated branch (never ever work in `master`)
4. Run `npm start` http://localhost:3000
5. Fix bugs or implement features
6. Always write tests
7. Format with Prettier `npm run format` and check ESLint `npm run lint`

Run tests:

```console
make test
```

[1]: https://github.com/AdeleD/react-paginate/blob/master/demo/js/demo.js
