### Cascade Types

While configuring collections in hbm file, also in mapping one-to-many, many-to-many mappings, the collection element (say, list) in the hbm file contains an attribute cascade.

> [!NOTE]  
> The cascade type can also be mentioned in an annotation. We will see that in the next section.

Hibernate provides several types of cascade options that can be used to manage the relationships between entities. Here are the different cascade types in Hibernate:

| Cascade Type            | Description                                                                                                                                                                    |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CascadeType.ALL         | A cascading type in Hibernate that specifies that all state transitions (create, update, delete, and refresh) should be cascaded from the parent entity to the child entities. |
| CascadeType.PERSIST     | A cascading type in Hibernate that specifies that the create (or persist) operation should be cascaded from the parent entity to the child entities.                           |
| CascadeType.MERGE       | A cascading type in Hibernate that specifies that the update (or merge) operation should be cascaded from the parent entity to the child entities.                             |
| CascadeType.REMOVE      | A cascading type in Hibernate that specifies that the delete operation should be cascaded from the parent entity to the child entities.                                        |
| CascadeType.REFRESH     | A cascading type in Hibernate that specifies that the refresh operation should be cascaded from the parent entity to the child entities.                                       |
| CascadeType.DETACH      | A cascading type in Hibernate that specifies that the detach operation should be cascaded from the parent entity to the child entities.                                        |
| CascadeType.REPLICATE   | A cascading type in Hibernate that specifies that the replicate operation should be cascaded from the parent entity to the child entities.                                     |
| CascadeType.SAVE_UPDATE | A cascading type in Hibernate that specifies that the save or update operation should be cascaded from the parent entity to the child entities.                                |

