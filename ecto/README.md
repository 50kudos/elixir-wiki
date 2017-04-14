### Starter diagram

This supposed to be a cheatsheet. Therefore, for complete details, please check the official document.

Ecto.Changeset is a data concern module. It is almost 100% separated from Repo which deals with database layer with adapter.
It will never hit database calls _unless_ you need to check some constraints such as unique_constraint.

Example of type signature of Repo.insert:
```elixir
insert(struct_or_changeset :: Ecto.Schema.t | Ecto.Changeset.t, opts :: Keyword.t) ::
  {:ok, Ecto.Schema.t} |
  {:error, Ecto.Changeset.t}
```

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
