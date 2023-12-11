# SalesOrder
En Microsoft Dynamics 365 for Finance and Operations, un “SalesOrder” és un objecte que es crea per vendre 
productes o serveis a un client. Juga un paper clau en el procés de venda, ja que permet fer un seguiment de les vendes 
des de la sol·licitud inicial fins al lliurament dels béns o serveis.

## Exemple
```
/// <summary>
/// It handles the creation of a sales order with a single sales line.
/// </summary>
class TRNCreateSalesOrder
{
  /// <summary>
  /// Class entry point. The system will call this method when a designated menu is selected 
  /// or when execution starts and this class is set as the startup class.
  /// </summary>
  /// <param name="_args"> Command line arguments passed to the class. </param>
  public static void main(Args _args)
  {
    NumberSeq   numberSeq;      // Variable to hold number sequence.
    SalesTable  salesTable;     // Table buffer for the SalesTable.
    SalesLine   salesLine;      // Table buffer for the SalesLine.

    ttsbegin;                   // Begin a transaction.

    // Retrieve a new number sequence for the sales order ID.
    numberSeq = NumberSeq::newGetNum(SalesParameters::numRefSalesId());
    numberSeq.used();           // Mark the number sequence as used.

    salesTable.SalesId = numberSeq.num();   // Initialize the sales order header with the new sales order ID.
    salesTable.initValue();                 // Initialize default values for the sales order.
    salesTable.CustAccount = 'US-002';      // Set the customer account number.
    salesTable.initFromCustTable();         // Initialize from related customer table.

    // Validate the sales order header before writing to the database.
    if (!salesTable.validateWrite())
    {
      throw Exception::Error;             // Throw an error if validation fails.
    }

    salesTable.insert();                    // Insert the sales order header into the database.
        
    salesLine.SalesId = salesTable.SalesId; // Initialize the sales line with the sales order ID.
    salesLine.ItemId = 'D0001';             // Set the item ID for the sales line.

    // Create the sales line with inventory, ledger, and other updates.
    salesLine.createLine(true, true, true, true, true, true);

    ttscommit;                  // Commit the transaction.

    // Display information message with the created sales order ID.
    info(strFmt('Sales order %1 has been created', salesTable.SalesId));
    }
}
```
