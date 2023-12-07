# RecordInsertList

Aquesta classe proporciona la capacitat d'inserir vectors directament al kernel. 
D'aquesta forma es pot inserir més d'un registre a la base de dades a la vegada reduïnt la comunicació entre 
l'aplicació i el servidor de base de dades.

## Exemple

```
/// <summary>
/// This class insert records in DestinationTable
/// </summary>
class Insert_RecordInsertList
{

  /// <summary>
  /// Class entry point. The system will call this method when a designated menu
  /// is selected or when execution starts and this class is set as the startup class.
  /// </summary>
  /// <param name = "_args"> The specified arguments. </param>
  public static void main(Args _args)
  { 

    // Declare instances of the tables.
    OriginTable1            varOriginTable1;
    OriginTable2            varOriginTable2;
    DestinationTable        varDestinationTable;
    RecordInsertList        varRecordInsertList;

    // Declare new RecordInsertList.
    varRecordInsertList = new RecordInsertList(tableNum(DestinationTable));

    //Insert data into the RecordInsertList.
    while select Field1, Field2 from varOriginTable1
      group by Field1, Field2
      join sum(Field3) from varOriginTable2
    where varOriginTable1.Field1 == varOriginTable2.Field1
    {
      varDestinationTable.initValue();
      varDestinationTable.Field1 = varOriginTable1.Field1;
      varDestinationTable.Field2 = varOriginTable1.Field2;
      varDestinationTable.Field3 = varOriginTable2.Field3;
      varRecordInsertList.Add(varDestinationTable); 
    }

    // Insert data into database.
    varRecordInsertList.insertDatabase();
  }
}
```
