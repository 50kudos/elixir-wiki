```ascii
+----------------+        +----------------+
| Ecto.Changeset |        |      Repo      |
+----------------+        +----------------+    (struct_or_changeset)
|cast, get, put, +-------->insert, delete, +-----------+
|delete, validate|  +----->update, etc     |           |
|etc.            |  |     |                |           |
+---^------------+  |     +----------------+      +----v-----+
    |               |                             | Database |
    |            no |     +----------------+      +----+-----+
    |        validation   |      Repo      |           |
    |               |     +----------------+           |
    |               |     |all, get, one,  <-----------+
   ++------------+  |     |delete_all,     |   Ecto.Query
   | Ecto.Schema +--+     |update_all, etc |    (queryable)
   +-------------+        +----------------+
      (struct)
```
