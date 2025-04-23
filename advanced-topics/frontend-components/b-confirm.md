---
icon: circle-check
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

# b-confirm

A dialog that is used to confirm an action.

### Usage:

```javascript
page.confirm(message, yesFunction[, noFunction[, cancelFunction]])[.setTimer(seconds)];
// or
bConfirm.confirm(message, yesFunction[, noFunction[, cancelFunction]])[.setTimer(seconds)];
```

### Arguments:

* _**message**_ - the text that will be shown in the header of the dialog.
* _**yesFunction**_ - executed after pressing the Yes button.
* _**noFunction**_ - executed after pressing the No button.
* _**cancelFunction**_ - executed after pressing the Cancel button. If this argument is not specified, there will be two buttons (Yes & No).

### Properties:

* The _**setTimer**_ function sets the timer to the Yes button and the button will be disabled until the time expires.

### Example:

```javascript
page.confirm('Do you want to save?', function() {
  ...saving
}).setTimer(3);
```
