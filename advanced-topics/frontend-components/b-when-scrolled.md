---
icon: scroll
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# b-when-scrolled

* Argument: Call back function which works element scroll is scrolled to end(95%).
*   Usage: to create infinite pagination

    Example:

    ```html
    <ul b-when-scrolled="loadMore()">
      <li> item1 </li>
      <li> item2 </li>
      ...
      <li> item-n </li>
    </ul>
    ```
*   Now this directive used for `b-input` and `b-tree-select` directives to auto load data and removed `show more` button:

    ```html
    ...
      <div class="hint-body" b-when-scrolled="_$bInput.hasMoreRows && _$bInput.onMoreClick()">
    ...
    ```
*   Codebase:

    ```javascript
    biruni.directive('bWhenScrolled', function () {
        return {
            restrict: 'A',
            scope: true,
            link: function (scope, elm, attr) {
                let raw = elm[0];
                if (attr.bWhenScrolled) {
                    function run() {
                        if (raw.scrollTop + raw.offsetHeight >= 0.95 * raw.scrollHeight) {
                            scope.$apply(attr.bWhenScrolled);
                        }
                    }

                    elm.bind('scroll', run);
                    elm.bind('touchmove', run);
                }
            }
        }
    });
    ```
