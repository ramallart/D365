# Update_recordset

La paraula clau ```update recordset``` s'utilitza quan es necessita actualitzar múltiples registres a una taula existent.
Aquesta instrucció és extremadament ràpida, ja que genera una única sentència SQL que s'envia al servidor una sola vegada.

Funciona al costat del servidor de base de dades en un conjunt de registres d'estil SQL, en lloc de recuperar cada
registre per separat mitjançant la recuperació, canvi i actualització.

## Exemple

```
/// <summary>
/// This class insert records in TRNBuildingContractTable
/// </summary>
class Update_recordset
{

  /// <summary>
  /// Class entry point. The system will call this method when a designated menu
  /// is selected or when execution starts and this class is set as the startup class.
  /// </summary>
  /// <param name = "_args"> The specified arguments. </param>
  public static void main(Args _args)
  { 

    // Declare instances of the tables.
    OriginTable         varOriginTable;
    DestinationTable    varDestinationTable;

    // Set update1
    update_recordset varDestinationTable
      setting Field1 = varDestinationTable.Field2/varDestinationTable.Field3;

    // Set update2
    update_recordset varDestinationTable
      setting Field4 = varOriginTable.Field1 + ', ' + varOriginTable.Field2
        join Field1, Field2 from varOriginTable
        where varDestinationTable.FieldId == varOriginTable.FieldId;
  }
}
```
