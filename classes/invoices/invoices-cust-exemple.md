# custInvoice
Una factura de client per a un SalesOrder és una factura en relació amb una venda que una organització dóna a un client. Aquest tipus 
de factura de client es crea basant-se en un SalesOrder, que inclou línies de comanda i números d’article. Les factures de client es 
poden utilitzar per verificar les factures del client, ja que es pot comparar la informació de la factura amb la informació del 
SalesOrder per assegurar-se que els càrrecs són correctes.

## Exemple
```
class CreateInvoice
{
  /// <summary>
  /// Class entry point. The system will call this method when a designated menu
  /// is selected or when execution starts and this class is set as the startup class.
  /// </summary>
  /// <param name = "_args">The specified arguments.</param>
  public static void main(Args _args)
  {
    // Define variables
    SalesFormLetter     salesFormLetter;
    SalesTable          salesTable;
    SalesId             salesId = '000754'; // Sales ID
    CustInvoiceJour     custInvoiceJour;

    ttsBegin; // Begin a transaction
    
    // Find the sales table with the specified sales ID
    salesTable = SalesTable::find(salesId);

    // If the sales table exists and the sales status is 'Delivered'
    if (salesTable && salesTable.SalesStatus == SalesStatus::Delivered)
    {
      // Construct a new sales form letter for an invoice
      salesFormLetter = SalesFormLetter::construct(DocumentStatus::Invoice);
      
      // Update the sales form letter with the information from the sales table
      salesFormLetter.update(salesTable, 
        systemDateGet(), // System date
        SalesUpdate::All, // Update all fields
        AccountOrder::None, // No account order
        NoYes::No, NoYes::No, // Do not update customer account or project account
        NoYes::No, // Do not update delivery account
        NoYes::Yes); // Update invoice account

      // If the journal record of the sales form letter is a customer invoice
      if (salesFormLetter.parmJournalRecord().TableId == tableNum(CustInvoiceJour))
      {
        // Assign the journal record to the custInvoiceJour variable
        custInvoiceJour = salesFormLetter.parmJournalRecord();
        
        // Display an info message with the sales ID and invoice ID
        info(strFmt('Sales Order #:%1 has been successfully posted and Invoice #:%2 is created!', 
          custInvoiceJour.SalesId, custInvoiceJour.InvoiceId));
      }
    }
    else
    {
      // If the sales table does not exist or is null, display an error message
      info(strFmt('%1 does not exist or is null in the system, please try again!', salesId));
    }
    
    ttscommit; // Commit the transaction
  }
}
```
