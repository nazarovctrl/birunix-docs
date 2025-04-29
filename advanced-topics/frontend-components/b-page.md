---
icon: memo
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

# b-page

This factory contains main functionality for page.

***

```
page.url('route name without colon', parameters);
```

This function returns a URL with parameters given by data.

Usage example

```
window.open(page.url('run', { id: 1 }));
```

***

```
page.uploadParamsUrl('route name without colon', parameters);
```

This function uploads parameters to the DB and returns a promise. On success, the URL is returned with the identification of uploaded data as a parameter.

Usage example

```
page.uploadParamsUrl('run', { id: 1 }).then(window.open);
```

When this function is used, at the backend package of this route actual parameters should be loaded.

Example of loading actual data:

```
Procedure Run(p Hashmap) is
    v_Actual_Data Hashmap := b.Get_Url_Params(p);
begin
    ...
end;
```
