---
icon: database
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

# Database Connection Class

This class is used to get connection to database. There two types of connection:

```
* Singleton connection
* Pool connection
```

When **getSingletonConnection()** method called application each time creates new connection with properties. It's not recommended to use this method because of resource usage.

When **getPoolConnection()** method called application gets connection from pool. It's recommended to use this method, because while getting connection from pool application doesn't create new connection, it gets connection from pool which already established connection with database. But while using Pool Connection you should close connection after using it to return connection to pool. Be careful, pool connection might have cashed data which was left after previous usage. To clear cashed data you should call **freeAllResources()** method. This method executes **Dbms\_Session.Modify\_Package\_State(Dbms\_Session.Free\_All\_Resources)** procedure.

If you wan fresh connection without cashed data then call **getPoolConnectionAndFreeResources()** method. This method executes **Dbms\_Session.Modify\_Package\_State(Dbms\_Session.Free\_All\_Resources)** procedure before return connection from pool.
