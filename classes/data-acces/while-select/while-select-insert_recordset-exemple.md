# Insert_recordset

La paraula clau ```insert_recordset``` s'utilitza quan necessites copiar múltiples 
registres de taules existents a una taula de destinació. És extremadament ràpid ja que genera una única sentència de SQL
que s'envia al servidor una sola vegada.

## Exemple
```
/// <summary>
/// This class insert records in DestinationTable
/// </summary>

class Insert_recordset
{
  /// <summary>
  /// Class entry point. The system will call this method when a designated menu
  /// is selected or when execution starts and this class is set as the startup class.
  /// </summary>
  /// <param name = "_args"> The specified arguments. </param>

  public static void main(Args _args)
  { 
    // Declare instances of the tables.
    OriginTable1        varOriginTable1;
    OriginTable2        varOriginTable2;
    DestinationTable    varDestinationTable;

    // Declare a temporary table
    TmpDestinationTable   tmpTable;

    // Insert data into the temporary table with group by
    while select Field1, Field2 from varOriginTable1
      join sum(Field3) from varOriginTable2
      group by varOriginTable1.Field1, varOriginTable1.Field2
      where varOriginTable1.Field1 == varOriginTable2.Field1
            
    {
      tmpTable.Field1 = varOriginTable1.Field1;
      tmpTable.Field2 = varOriginTable1.Field2;
      tmpTable.Field3 = varOriginTable2.Field3;
      tmpTable.insert();
    }

    // Insert data from the temporary table into the target table
    insert_recordset varDestinationTable (Field1, Field2, Field3)
      select Field1, Field2, Field3 from tmpTable;
    }
}

```
