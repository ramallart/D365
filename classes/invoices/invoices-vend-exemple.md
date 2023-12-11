# VendInvoice
 Les factures de proveïdor són sol·licituds de pagament rebudes per a productes i serveis. Les factures de proveïdor poden 
 representar un compte per a serveis continus o es poden basar en PurchOrders per a articles i serveis específics. Les factures de 
 proveïdor es poden utilitzar per verificar les factures del proveïdor, ja que es pot comparar la informació de la factura amb la 
 informació del PurchOrder per assegurar-se que els càrrecs són correctes.

 ## Important: Three-way matching
 Dynamics 365 té, per defecte, una política de comprovació dels items de les PurchOrder. Per a desenvolupar de forma correcta una VendInvoice, 
 necessitem desactivar-la. Per fer-ho farem el següent pathing:
  
> Accounts payable -> Setup -> Accounts payable parameters -> Invoice validation -> Enable invoice matching validation -> No

## Exemple
```
class CreateInvoicePurch
{
  // The main method is the entry point of the class.
  public static void main(Args _args)
  {
    // Define variables
    PurchFormLetter     purchFormLetter;
    PurchTable          purchTable;
    PurchId             purchId = '000026'; // Purchase ID
    VendInvoiceJour     vendInvoiceJour;

    ttsbegin; // Begin a transaction

    // Find the purchase table with the specified purchase ID
    purchTable = PurchTable::find(purchId);

    // Construct a new purchase form letter for an invoice
    purchFormLetter = PurchFormLetter::construct(DocumentStatus::Invoice);
        
    // Update the purchase form letter with the information from the purchase table
    purchFormLetter.update(purchTable, 'Inv_'+purchTable.PurchId, systemDateGet());

    // Assign the journal record of the purchase form letter to the vendInvoiceJour variable
    vendInvoiceJour = purchFormLetter.parmJournalRecord();

    // Display an info message with the purchase ID and invoice ID
    info(strFmt('Purch Order #:%1 has been successfully posted and Invoice #:%2 is created!',
      vendInvoiceJour.PurchId, vendInvoiceJour.InvoiceId));

    ttscommit; // Commit the transaction
    }
}
```
