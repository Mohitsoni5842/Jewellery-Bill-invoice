Jewellery Billing Application
This application is a simple billing system designed for a jewellery store. It allows you to add items, calculate the total amount, generate an invoice, and print the bill as a PDF.

Features
Add Items: Enter the item name, weight in grams, rate per gram, and making charge to calculate the total amount.
Item List: View all added items in a Treeview with columns for item name, weight, rate per gram, making charge, and the calculated amount.
Generate Bill: Create a detailed bill with customer information and itemized details.
Print Bill: Save the bill as a PDF file and open it for printing or saving.
Requirements
Python 3.x
Tkinter
PIL (Pillow)
FPDF
You can install the required packages using pip:

bash
Copy code
pip install pillow fpdf2
How to Use
Run the Application: Start the application by running the Python script.
Open the Bill Window: The billing window will open automatically.
Add Customer Details: Enter the customer's name and phone number in the respective fields.
Add Items: Input the item name, weight in grams, rate per gram, and making charge, then click the "Add Item" button. The item will be added to the list, and the total amount will be updated.
Generate Bill: Once all items are added, click the "Generate Bill" button to view the bill in a new window.
Print Bill: Click the "Print Bill" button to generate a PDF of the bill and open it for viewing or printing.
Code Overview
open_bill_window()
This function creates the main billing window, allowing users to input customer details and item information. It includes:

Input Fields: For customer details, item name, weight, rate per gram, and making charge.
Treeview: A table that displays the list of added items with their details and calculated amount.
Buttons: For adding items, generating the bill, and printing the bill.
add_item()
This function is triggered by the "Add Item" button. It validates the input, calculates the amount for the item, and updates the total amount.

update_total()
This function calculates and updates the total amount based on the items added to the Treeview.

generate_bill()
This function creates a new window displaying the generated bill with all customer and item details.

generate_bill_text()
This function generates the text content for the bill, which is used by the print_bill() function.

print_bill()
This function generates a PDF version of the bill and opens it for viewing or printing.

Notes
The application uses os.startfile("bill.pdf", "open") to open the PDF file. This command may vary depending on the operating system. Ensure that your system has a default PDF viewer associated with .pdf files.
Troubleshooting
ValueError in Weight, Rate, or Making Charge: Ensure that only numerical values are entered in these fields.
Bill PDF Not Opening: Check if you have a default PDF viewer installed and properly associated with .pdf files.
Future Enhancements
Add the ability to save customer details for future reference.
Implement discount functionality.
Integrate a database for storing transactions.
