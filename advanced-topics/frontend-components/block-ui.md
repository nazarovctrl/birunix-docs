---
icon: lock
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

# Block-UI

Block UI blocks screen and show loading animation. In order to use Block-UI in html forms require blockUI attribute in page controller. Read this for more information [angular-block-ui](https://github.com/McNull/angular-block-ui).

```javascript
  page.ctrl(function(scope, blockUI) {

  });
```

Block and show loading animation

```
  blockUI.start();
```

Unblock and hide loading animation

```javascript
  blockUI.stop();

  // or

  scope.$apply(function() {
    blockUI.stop();
  });
```

It is recommended to check before unblock

```javascript
  if (blockUI.state().blocking) {
    scope.$apply(function() {
      blockUI.stop();
    });
  }
```
