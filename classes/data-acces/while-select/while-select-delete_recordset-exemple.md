# Delete_recordset

La paraula clau ```delete_from``` s’utilitza quan necessites esborrar múltiples registres d’una taula existent. 
Aquesta instrucció és extremadament ràpida, ja que genera una única sentència SQL que s’envia al servidor una sola vegada.

Funciona al costat del servidor de base de dades en un conjunt de registres d’estil SQL, en lloc de recuperar cada registre per 
separat mitjançant la recuperació, canvi i actualització.

## Exemple
```
/// <summary>
/// This class insert records in TRNBuildingContractTable
/// </summary>
class Delete_recordset
{

  /// <summary>
  /// Class entry point. The system will call this method when a designated menu
  /// is selected or when execution starts and this class is set as the startup class.
  /// </summary>
  /// <param name = "_args"> The specified arguments. </param>
  public static void main(Args _args)
  { 

    // Declare instances of the tables.
    DestinationTable    varDestinationTable;

    delete_from varDestinationTable
      where varDestinationTable.Field1 == someValue;
  }
}
```
