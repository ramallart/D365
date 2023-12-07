# SysDaUpdateStatement

La classe ```SysDaUpdateStatement``` en X++ és part de l’API SysDa, que permet crear 
consultes personalitzades per actualitzar dades en una taula. Aquesta classe 
s’utilitza quan es necessita modificar múltiples registres d’una taula existent.

## Exemple 1
```
class SysDaUpdateStatement
{       
  public static void main(Args _args)
  {
    // Declare the buffer for the origin table
    UpdateTable    varUpdateTable;

    // Create a SysDaUpdateObject for the varUpdateTable
    var up = new SysDaUpdateObject(varUpdateTable);

    // Add a setting clause to update the Field1 with the result of dividing the Field2 by the Field3
    up.settingClause()
      .add(fieldStr(UpdateTable, Field1), new SysDaDivideExpression(
        new SysDaFieldExpression(varUpdateTable, fieldStr(UpdateTable, Field2)),
        new SysDaFieldExpression(varUpdateTable, fieldStr(UpdateTable, Field3))));

    // Start a transaction
    ttsbegin;

      // Execute the update statement on the database
      new SysDaUpdateStatement().update(up);

    // Commit the transaction
    ttscommit;
  }
}
```

## Exemple 2
```
class SysDaUpdateStatement
{       
  public static void main(Args _args)
  {
    // Declare the buffers for the origin and update tables
    OriginTable    varOriginTable;
    UpdateTable    varUpdateTable;

    // Create a SysDaUpdateObject for the update table
    var up = new SysDaUpdateObject(varUpdateTable);

    // Create a SysDaQueryObject for the origin table
    var upq = new SysDaQueryObject(varOriginTable);

    // Add a projection clause to select the Field1 and Field2 from the origin table
    upq.projection()
      .add(fieldStr(OriginTable, Field1))
      .add(fieldStr(OriginTable, Field2));

    // Add a join clause to join the update table with the origin table
    up.joinClause(SysDaJoinKind::InnerJoin, upq);

    // Add a setting clause to update the Field3 of the update table with the sum of Field1 and Field2 from the origin table
    up.settingClause()
      .add(fieldStr(UpdateTable, Field3), new SysDaPlusExpression(
        new SysDaFieldExpression(varUpdateTable, fieldStr(OriginTable, Field1)),
        new SysDaFieldExpression(varUpdateTable, fieldStr(OriginTable, Field2))));

    // Start a transaction
    ttsbegin;

      // Execute the update statement on the database
      new SysDaUpdateStatement().update(up);

    // Commit the transaction
    ttscommit;
  }
}
```
