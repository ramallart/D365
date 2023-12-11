# Insert_RecordSet
La classe ```Query::insert_recordset``` a Dynamics 365 Finance and Operations és una eina molt potent que permet copiar dades 
directament des d’una o més taules d’origen a una taula de destinació en un sol viatge al servidor. Aquesta classe és especialment útil 
quan necessites copiar múltiples registres des de taules existents a una taula de destinació.

## Exemple
```
/// <summary>
/// This class insert records in DestinatTable.
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
    // Declare instance of the table.
    DestinationTable    varDestinationTable;

    // Declare a query object.
    Query query;

    // Create a new query object.
    query = new Query();
    query.clearAllFields();

    // Add OriginTable1 as a data source to the query.
    QueryBuildDataSource qdbOrigin1 = query.addDataSource(tableNum(OriginTable1));

    // Add the OriginTable1 selection fields.
    qdbOrigin1.addGroupByAndSelectionField(fieldNum(OriginTable1, Field1));
    qdbOrigin1.addGroupByAndSelectionField(fieldNum(OriginTable1, Field2));

    // Add OringinTable2 as a data source to the query and join it with OriginTable1.
    QueryBuildDataSource qdbOrigin2 = qdbOrigin1.addDataSource(tableNum(OringinTable2));
    qdbOrigin2.relations(true); // Enable relations.

    // Add the OringinTable2 selection fields.
    qdbOrigin2.addSelectionField(fieldNum(OringinTable2,Field3), SelectionField::Sum);


    // Create a map to hold the target to source field mappings.
    Map targetToSourceMap = new Map(Types::String, Types::Container);
    targetToSourceMap.insert(fieldStr(DestinationTable, Field1),
      [qdbOrigin1.uniqueId(), fieldStr(OringinTable1, Field1)]);
    targetToSourceMap.insert(fieldStr(DestinationTable, Field2),
      [qdbOrigin1.uniqueId(), strFmt('%1', fieldStr(OringinTable1, Field2))]);
    targetToSourceMap.insert(fieldStr(DestinationTable, Field3),
      [qdbOrigin2.uniqueId(), strFmt('SUM(%1)', fieldStr(OringinTable2, Field3))]);

    ttsbegin;   // Begin a transaction.

    // Insert records into buildingContractTable from the query result.
    Query::insert_recordset(varDestinationtable, targetToSourceMap, query);

    ttscommit;  // End the transaction.
  }
}
```
