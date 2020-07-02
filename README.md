# Automated and Send Out Invoices from CRM
The main idea here is to, at some defined trigger in the CRM, automatically create and invoice and send it out via email.

## Configuration
The trigger needs to be set up, probably via a workflow rule or blueprint transition, i.e. when a "Deal" is marked 'Closed', trigger this function. The defined argument in this function is the Contact_ID, but you can query that from whichever module you start the function. Below, I talk more about options for adding line items.
You will have to create the connection between CRM and the Finance Suite. In CRM, go to Setup >> Developer Space >> Connections

## Walkthrough
The first few lines define variables needed in the rest of the function, pulling information from the (CRM) Contact record and using the Zoho Books Organizations API to get the Organization ID. This is frequently used with Deluge commands in Books.
A quick search checks the Customers (but the API name is "contacts") module for our email. If it is empty, we go ahead and create the Customer, and if not, we just pull the Contact Person information for the Invoice.
We create maps for the new invoice and add the payment options to that. Here, there are many ways to customize this function further. Some ideas are to create a sync between Books-CRM and the Items and Products module. Then, you can choose the products in CRM and via a query to the Items module (in Books), add those line items to the Invoice. You can simply leave it as it is and specify a single item if needed. You can change the payment options.
After the Invoice is created successfully, `if(code==0)`, the last API call sends off the email with the created Invoice

