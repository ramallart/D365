# SysDaDeleteStatement
La classe ```SysDaDeleteStatement``` és part de l’API SysDa en Dynamics 365 Finance and Operations. 
S’utilitza per a operacions de supressió de dades. Juntament amb ```SysDaQueryObject``` i ```SysDaDeleteObject```, 
aquestes classes permeten crear consultes personalitzades per a eliminar dades.

## Exemple

```
class SysDaDeleteStatement
{
  public static void main(Args _args)
  {
    // Declare a variable of type DeleteTable
    DeleteTable    varDeleteTable;

    // Create a new SysDaQueryObject with varDeleteTable as the argument
    var qe = new SysDaQueryObject(varDeleteTable);

    // Add the field 'Field' from the DeleteTable to the projection of the query
    qe.projection()
      .add(fieldStr(DeleteTable, Field));

    // Add a where clause to the query that checks if the 'Field' of varDeleteTable equals 'SomeValue'
    qe.whereClause(new SysDaEqualsExpression(
      new SysDaFieldExpression(varDeleteTable, fieldStr(DeleteTable, Field)), new SysDaValueExpression(SomeValue)));

    // Create a new SysDaDeleteStatement
    var dlt = new SysDaDeleteStatement();

    // Create a new SysDaDeleteObject with the query expression
    var dltObject = new SysDaDeleteObject(qe);

    // Begin a transaction
    ttsbegin;

    // Execute the delete statement
    dlt.delete(dltObject);

    // Commit the transaction
    ttscommit;

  }
}
```
