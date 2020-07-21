# Automate and Send Out Invoices from CRM
The main idea here is, at some defined trigger in the CRM, to automatically create an invoice for a specific Contact and send it to them via email.

## Configuration
The trigger needs to be set up via a workflow rule or blueprint transition, i.e. when a "Deal" is marked 'Closed'. The defined argument in this function is the Contact_ID, but you can query that from whichever module you start the function. Below, I talk more about options for adding line items.

You will have to create the connection between CRM and the Finance Suite. In CRM, go to Setup >> Developer Space >> Connections to create and name the connection.

## Walkthrough
The first few lines define variables needed in the rest of the function, pulling information from the (CRM) Contact record and using the Zoho Books Organizations API to get the Organization ID. This field is frequently used with Deluge commands in the Zoho Finance Suite. Zoho provided the ability to create and manage multiple organizations in Books, and this specific ID is used in most Deluge commands in Books.

A quick search checks the Customers (but the API name is "contacts") module for the contact's email. If it is empty, we go ahead and create the Customer, and if not, we just pull the Contact Person information for the Invoice. This comprises lines 24-56. 

Next, we create maps for the new invoice and add the payment options to that. Here, there are many ways to customize this function further. Some ideas are to create a sync between Books-CRM and the Items and Products module. Then, you can choose the products in CRM and via a query to the Items module (in Books), add those line items to the Invoice. You can simply leave it as it is and specify a single item if needed (as is done here). You can also customize the payment options. A useful resource is the Zoho Books API Doc, specifically the `zoho.books.createRecord` command. Then, to better learn how to add custom fields, refer to our "Zoho Books Custom Fields" repository.

After the Invoice is created successfully, `if(code==0)`, the last API call sends off the email with the created Invoice (also detailed in the Zoho Books API doc).

