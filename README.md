import tkinter as tk
from tkinter import ttk, messagebox
from PIL import Image, ImageTk
from fpdf import FPDF
import os

def open_bill_window():
    def add_item():
        try:
            item_name = item_name_entry.get()
            weight = float(weight_entry.get())
            rate = float(rate_entry.get())
            making_charge = float(making_charge_entry.get())
            amount = (weight * rate) + making_charge
            treeview.insert("", "end", values=(item_name, weight, rate, making_charge, amount))
            update_total()
        except ValueError:
            messagebox.showerror("Input Error", "Please enter valid numerical values for Weight, Rate, and Making Charge.")

    def update_total():
        total = 0
        for item in treeview.get_children():
            total += float(treeview.item(item, 'values')[4])
        total_label.config(text=f"Total: {total}")

    def generate_bill():
        bill_window = tk.Toplevel(root)
        bill_window.title("Bill")
        bill_text = tk.Text(bill_window)
        bill_text.pack(fill=tk.BOTH, expand=True)

        bill_text.insert(tk.END, "       Jewellery Bill\n")
        bill_text.insert(tk.END, "XYZ Jewellers\n")
        bill_text.insert(tk.END, "Address\n")
        bill_text.insert(tk.END, "Contact No.-9xxxxxxxxx\n")
        bill_text.insert(tk.END, "_______________________________________________________\n")
        bill_text.insert(tk.END, f"Customer Name: {customer_name_entry.get()}\n")
        bill_text.insert(tk.END, f"Customer No.: {customer_phone_entry.get()}\n")
        bill_text.insert(tk.END, "_______________________________________________________\n")
        bill_text.insert(tk.END, "Item Name Weight(g) Rate/gram Making charge Amount\n")
        for item in treeview.get_children():
            item_values = treeview.item(item, 'values')
            bill_text.insert(tk.END, f"{item_values[0]}|    {item_values[1]}g|    {item_values[2]}/g    {item_values[3]}            {item_values[4]}\n")
    
        bill_text.insert(tk.END, f"Total: {total_label.cget('text')}\n")
        bill_text.insert(tk.END,"Thanks For Visit\n")
        bill_text.config(state=tk.DISABLED)

    def generate_bill_text():
        bill_text = "                            XYZ Jewellers \n"
        bill_text += f"Address: Jaipur\n"
        bill_text += f"Phone Number: 9xxxxxxxxx\n"
        bill_text += "____________________________________________\n"
        bill_text += f"Customer Name: {customer_name_entry.get()}\n"
        bill_text += f"Customer Phone Number: {customer_phone_entry.get()}\n"
        bill_text += "_____________________________________________\n"
        bill_text += f"Item Weight(g) Rate/gram Making charge Amount\n"
        for item in treeview.get_children():
            item_values = treeview.item(item, 'values')
            bill_text += f"{item_values[0]}     {item_values[1]}g    {item_values[2]}/g   {item_values[3]}  {item_values[4]}\n"
        bill_text += "_____________________________________________\n"
        bill_text += f"Total: {total_label.cget('text').split(': ')[1]}\n"
        bill_text += "_____________________________________________\n"
        bill_text += f"                         Thanks For Visit\n"
        return bill_text

    def print_bill():
        bill_text = generate_bill_text()
        if bill_text:
            try:
                pdf = FPDF()
                pdf.add_page()
                pdf.set_font("Arial", size=12)
                for line in bill_text.split('\n'):
                    pdf.cell(200, 10, txt=line, ln=True)
                pdf.output("bill.pdf")
                os.startfile("bill.pdf", "open")  # Use "open" instead of "print" to just open the file
                messagebox.showinfo("Success", "Bill generated successfully")
            except Exception as e:
                messagebox.showerror("Error", f"Failed to generate bill: {e}")

    bill_window = tk.Toplevel(root)
    bill_window.title("Bill")
    bill_window.geometry(f"{screen_width}x{screen_height}")
    bill_label = tk.Label(bill_window, text="Invoice", bg="cadet blue", font=("Helvetica", 22, "bold"))
    bill_label.pack(ipadx=700)
    purchase_frame = tk.Frame(bill_window, bg="navajo white")
    purchase_frame.pack(pady=10, side=tk.LEFT, fill=tk.BOTH, expand=True)

    tk.Label(purchase_frame, text="Customer Name", bg="navajo white", font=("Helvetica", 15, "bold")).grid(row=5, column=0, padx=1, pady=5)
    customer_name_entry = tk.Entry(purchase_frame, font=("Helvetica", 15, "bold"))
    customer_name_entry.grid(row=5, column=2, pady=5)

    tk.Label(purchase_frame, text="Customer Phone Number", bg="navajo white", font=("Helvetica", 15, "bold")).grid(row=6, column=0, padx=1, pady=5)
    customer_phone_entry = tk.Entry(purchase_frame, font=("Helvetica", 15, "bold"))
    customer_phone_entry.grid(row=6, column=2, pady=5)

    tk.Label(purchase_frame, text="Item Name", bg="navajo white", font=("Helvetica", 15, "bold")).grid(row=7, column=0, padx=1, pady=5)
    item_name_entry = tk.Entry(purchase_frame, font=("Helvetica", 15, "bold"))
    item_name_entry.grid(row=7, column=2, pady=5)

    tk.Label(purchase_frame, text="Weight (g)", bg="navajo white", font=("Helvetica", 15, "bold")).grid(row=8, column=0, padx=1, pady=5)
    weight_entry = tk.Entry(purchase_frame, font=("Helvetica", 15, "bold"))
    weight_entry.grid(row=8, column=2, pady=5)

    tk.Label(purchase_frame, text="Rate per gram", bg="navajo white", font=("Helvetica", 15, "bold")).grid(row=9, column=0, pady=5)
    rate_entry = tk.Entry(purchase_frame, font=("Helvetica", 15, "bold"))
    rate_entry.grid(row=9, column=2, pady=5)

    tk.Label(purchase_frame, text="Making Charge", bg="navajo white", font=("Helvetica", 15, "bold")).grid(row=10, column=0, padx=1, pady=5)
    making_charge_entry = tk.Entry(purchase_frame, font=("Helvetica", 15, "bold"))
    making_charge_entry.grid(row=10, column=2, pady=5)

    add_button = tk.Button(purchase_frame, text="Add Item", bg="cadet blue", font=("Helvetica", 15, "bold"), command=add_item)
    add_button.grid(row=11, column=1)

    columns = ("Item Name", "Weight", "Rate per gram", "Making Charge", "Amount")
    treeview = ttk.Treeview(purchase_frame, columns=columns, show="headings")
    treeview.heading("Item Name", text="Item Name")
    treeview.heading("Weight", text="Weight (g)")
    treeview.heading("Rate per gram", text="Rate per gram")
    treeview.heading("Making Charge", text="Making Charge")
    treeview.heading("Amount", text="Amount")
    treeview.grid(row=20, column=1, padx=1, pady=10, sticky="ew")

    total_label = tk.Label(purchase_frame, text="Total: 0", bg="navajo white", font=("Helvetica", 15, "bold"))
    total_label.grid(row=25, column=1, pady=10)

    generate_bill_button = tk.Button(purchase_frame, text="Generate Bill", bg="cadet blue", font=("Helvetica", 15, "bold"), command=generate_bill)
    generate_bill_button.grid(row=35, column=1, pady=10)

    print_bill_button = tk.Button(purchase_frame, text="Print Bill", bg="cadet blue", font=("Helvetica", 15, "bold"), command=print_bill)
    print_bill_button.grid(row=36, column=1, pady=10)

root = tk.Tk()
screen_width = root.winfo_screenwidth()
screen_height = root.winfo_screenheight()
open_bill_window()
root.mainloop()

