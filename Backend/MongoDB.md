#### mongoose 下的事务处理（transaction）

***session.withTransaction()** //// **Connections#transaction()**
- Create a transaction.
- Commiting the transaction if it succeeds.
- Absorting the transaction if your operation throws.
- Retry in the event of a transient transaction error.



### in nestjs

1. Controling the transaction

```
InjectConnection() connection: mongoose.Connection
session = connection.startSession();
session.startTransaction();
1. session.commitTransaction();
2. session.abortTransaction();
session.endSession();
```

2. simpler way of  using transactions
```
session.withTransaction(()=>{

})
session.endSession();
```