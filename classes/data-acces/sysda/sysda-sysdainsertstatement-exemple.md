# SysDaInsertStatement

Aquesta classe forma part de l'API SysDA (llegir sysda-explicació per a més informació).
Concretament ```SysDaInsertStatement``` s'utilitza per inserir dades en una taula.

## Exemple

```
class SysDaInsertStatement
{

  public static void main(Args _args)
  {
    OriginTable1        varOriginTable1;
    OriginTable2        varOriginTable2;
    DestinationTable    varDestinationTable;

    // Build a SysDaInsertObject with the detination of the data to insert.
    var insertObject = new SysDaInsertObject(varDestinationTable);

    insertObject.fields()
      .add(fieldStr(DestinationTable, Field1))
      .add(fieldStr(DestinationTable, Field2))
      .addSum(fieldStr(DestinationTable, Field3));

        // Build a SysDaQueryObject for buildingTable.
        var qb = new SysDaQueryObject(varOriginTable1);

        // Add TRNBuilding fields to the query.
        qb.projection()
            .add(fieldStr(OriginTable1, Field1))
            .add(fieldStr(OriginTable1, Field2));

        // Group fields.
        qb.groupByClause()
            .add(fieldStr(OriginTable1, Field1))
            .add(fieldStr(OriginTable1, Field2));

        // Build a SysDaQueryObject fot contractTable.
        var qc = new SysDaQueryObject(varOriginTable2);

        // Join the SysDaQueryObjects.
        qb.joinClause(SysDaJoinKind::InnerJoin, qc);

        // Add TRNContract field to the query.
        qc.projection()
            .addSum(fieldStr(OriginTable2, Field3));
        
        // Add whereClause to make the relation.
        qc.whereClause(new SysDaEqualsExpression(
            new SysDaFieldExpression(varOriginTable1, fieldStr(OriginTable1, FieldId)),
            new SysDaFieldExpression(varOriginTable2, fieldStr(OriginTable2, FieldId))));

        // Add the SysDaQueryObject to the insertObject.
        insertObject.query(qb);

        // Declare a new SysDaInsertStatement.
        var insertStmt = new SysDaInsertStatement();

        // Begin the transaction.
        ttsbegin;
        
        // Insert fields to the table.
        insertStmt.executeQuery(insertObject);
        
        // End the transaction.
        ttscommit;
  }
}
```
