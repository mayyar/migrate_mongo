# migrate_mongo

### Mongodump/Mongorestore
#### Getting the Source Data to Your Local Machine
```bash
$ mongodump --forceTableScan --uri="mongodb+srv://user_a:password_a@database_a.mongodb.net/db_1?ssl=true&authSource=admin" --collection="collection_b"
```
</br>

If you use an older mongo client (<4.0) to dump/export a more recent MongoDB you will get the error
> Unrecognized field 'snapshot'.

Using ```forceTableScan``` will prevent the snapshot functionality to become active, this you won't get the error

</br>

* If we look in our current directory, we can see the backup was created in the ```./dump``` directory:
```bash
$ tree
.
└── dump
    └── db_1
        ├── collection_b.bson
        └── collection_b.metadata.json
```

</br>

#### Pushing the Local Data to the Target Mongo Server
* Method 1 (Recommended)
    * Use mv to rename directory (i.e. DB name)
    ```bash
    $ mv crawler ffo
    ```
    * Use ```--nsInclude```, passing in the full namespace (<database>.<collection>) of the collection.
    ```bash
    $ mongorestore --uri="mongodb+srv://user_a:password_a@database_b.mongodb.net/db_1?ssl=true&authSource=admin" --nsInclude="db_1.collection_b" ./dump
    ```
* Method 2 (might have some insertion error)
    * Restore a specific collection using the --db, --collection, and a .bson file:
    ```bash
    $ mongorestore --uri="mongodb+srv://user_a:password_a@database_b.mongodb.net/db_1?ssl=true&authSource=admin" --db="db_1" --collection="collection_b" ./dump/db_1/collection_b.bson
    ```
