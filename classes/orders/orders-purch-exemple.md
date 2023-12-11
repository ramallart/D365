# PurchOrder
En Microsoft Dynamics 365 for Finance and Operations, un "PurchOrder" és una ordre que es crea per adquirir 
béns o serveis d'un proveïdor. Aquest objecte inclou detalls com ara els articles que es volen comprar, les quantitats,
els preus, el proveïdor del qual es volen adquirir els articles, la data de lliurament esperada, etc. Un cop creat es pot
enviar al proveïdor com a sol·licitut formal d'adquisició dels béns o serveis especificats. 

Per tant, juga un paper clau en el procés de compra, ja que permet fer un seguiment de les compres des de la sol·licitut 
inicial fins a la recepció dels béns o serveis.

## Exemple
```
/// <summary>
/// It handles the creation of a purchase order with a single purchase line.
/// </summary>
class TRNCreatePurchOrder
{
  /// <summary>
  /// Class entry point. The system will call this method when a designated menu is selected 
  /// or when execution starts and this class is set as the startup class. 
  /// </summary>
  /// <param name = "_args"> The specified arguments. </param>
  public static void main(Args _args)
  {
    NumberSeq   numberSeq;      // Variable to hold number sequence.
    PurchTable  purchTable;     // Table buffer for the PurchTable.
    PurchLine   purchLine;      // Table buffer for the PurchLine.

    ttsbegin;                   // Begin a transaction.

    // Retrieve a new number sequence for the purchase order ID.
    numberSeq = NumberSeq::newGetNum(PurchParameters::numRefPurchId());
    numberSeq.used();           // Mark the number sequence as used.

    purchTable.PurchId = numberSeq.num();       // Initialize the purchase order header with the new purchase order ID.
    purchTable.initValue();                     // Initialize default values for the purchase order.

    // Initialize from related vendor table using the vendor account number '1003'.
    purchTable.initFromVendTable(VendTable::find('1003'));

    // Validate the purchase order header before writing to the database.
    if(!purchTable.validateWrite())
    {
      throw Exception::Error;                 // Throw an error if validation fails.
    }

    purchTable.insert();                        // Insert the purchase order header into the database.
       
    purchLine.PurchId = purchTable.PurchId;     // Initialize the purchase line with the purchase order ID.
    purchLine.ItemId = 'D0001';                 // Set the item ID for the purchase line.

    // Create the purchase line with inventory, ledger, and other updates.
    purchLine.createLine(true, true, true, true, true, true);
 
    ttscommit;                  // Commit the transaction.

    // Display information message with the created purchase order ID.
    info(strFmt('Purchase order %1 has been created', purchTable.PurchId));  
    }
}
```
