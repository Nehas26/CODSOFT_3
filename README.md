# CODSOFT_3
import tkinter as tk
from tkinter import messagebox, simpledialog
import tkinter.ttk as ttk

# Initialize the contacts list (in-memory storage)
contacts = []

# Function to add a new contact
def add_contact():
    name = name_entry.get()
    phone = phone_entry.get()
    email = email_entry.get()
    address = address_entry.get()

    if name and phone:
        contact = {
            "name": name,
            "phone": phone,
            "email": email,
            "address": address
        }
        contacts.append(contact)
        refresh_contact_list()
        clear_entries()
    else:
        messagebox.showerror("Error", "Name and Phone are required fields.")

# Function to refresh the contact list in the Treeview
def refresh_contact_list():
    for row in contact_tree.get_children():
        contact_tree.delete(row)

    for idx, contact in enumerate(contacts):
        contact_tree.insert("", tk.END, iid=idx, values=(contact["name"], contact["phone"]))

# Function to clear entry fields after adding a contact
def clear_entries():
    name_entry.delete(0, tk.END)
    phone_entry.delete(0, tk.END)
    email_entry.delete(0, tk.END)
    address_entry.delete(0, tk.END)

# Function to search for a contact
def search_contact():
    search_term = search_entry.get().lower()
    for row in contact_tree.get_children():
        contact_tree.delete(row)

    for idx, contact in enumerate(contacts):
        if search_term in contact["name"].lower() or search_term in contact["phone"]:
            contact_tree.insert("", tk.END, iid=idx, values=(contact["name"], contact["phone"]))

# Function to update the selected contact
def update_contact():
    selected_item = contact_tree.selection()
    if selected_item:
        idx = int(selected_item[0])
        name = name_entry.get()
        phone = phone_entry.get()
        email = email_entry.get()
        address = address_entry.get()

        if name and phone:
            contacts[idx] = {"name": name, "phone": phone, "email": email, "address": address}
            refresh_contact_list()
            clear_entries()
        else:
            messagebox.showerror("Error", "Name and Phone are required fields.")
    else:
        messagebox.showerror("Error", "No contact selected.")

# Function to delete the selected contact
def delete_contact():
    selected_item = contact_tree.selection()
    if selected_item:
        idx = int(selected_item[0])
        del contacts[idx]
        refresh_contact_list()
        clear_entries()
    else:
        messagebox.showerror("Error", "No contact selected.")

# Function to populate fields when a contact is selected from the list
def on_select(event):
    selected_item = contact_tree.selection()
    if selected_item:
        idx = int(selected_item[0])
        contact = contacts[idx]
        name_entry.delete(0, tk.END)
        name_entry.insert(0, contact["name"])
        phone_entry.delete(0, tk.END)
        phone_entry.insert(0, contact["phone"])
        email_entry.delete(0, tk.END)
        email_entry.insert(0, contact["email"])
        address_entry.delete(0, tk.END)
        address_entry.insert(0, contact["address"])

# Initialize the Tkinter window
root = tk.Tk()
root.title("Contact Management System")
root.geometry("600x500")
root.config(bg="#f0f8ff")

# Contact form
form_frame = tk.Frame(root, bg="#f0f8ff")
form_frame.pack(pady=10)

tk.Label(form_frame, text="Name", font=("Arial", 12), bg="#f0f8ff").grid(row=0, column=0, padx=10, pady=5)
name_entry = tk.Entry(form_frame, font=("Arial", 12))
name_entry.grid(row=0, column=1, padx=10, pady=5)

tk.Label(form_frame, text="Phone", font=("Arial", 12), bg="#f0f8ff").grid(row=1, column=0, padx=10, pady=5)
phone_entry = tk.Entry(form_frame, font=("Arial", 12))
phone_entry.grid(row=1, column=1, padx=10, pady=5)

tk.Label(form_frame, text="Email", font=("Arial", 12), bg="#f0f8ff").grid(row=2, column=0, padx=10, pady=5)
email_entry = tk.Entry(form_frame, font=("Arial", 12))
email_entry.grid(row=2, column=1, padx=10, pady=5)

tk.Label(form_frame, text="Address", font=("Arial", 12), bg="#f0f8ff").grid(row=3, column=0, padx=10, pady=5)
address_entry = tk.Entry(form_frame, font=("Arial", 12))
address_entry.grid(row=3, column=1, padx=10, pady=5)

# Buttons for managing contacts
button_frame = tk.Frame(root, bg="#f0f8ff")
button_frame.pack(pady=10)

add_button = tk.Button(button_frame, text="Add Contact", font=("Arial", 12), command=add_contact)
add_button.grid(row=0, column=0, padx=10)

update_button = tk.Button(button_frame, text="Update Contact", font=("Arial", 12), command=update_contact)
update_button.grid(row=0, column=1, padx=10)

delete_button = tk.Button(button_frame, text="Delete Contact", font=("Arial", 12), command=delete_contact)
delete_button.grid(row=0, column=2, padx=10)

# Search bar
search_label = tk.Label(root, text="Search", font=("Arial", 12), bg="#f0f8ff")
search_label.pack(pady=5)

search_entry = tk.Entry(root, font=("Arial", 12))
search_entry.pack(pady=5)

search_button = tk.Button(root, text="Search Contact", font=("Arial", 12), command=search_contact)
search_button.pack(pady=5)

# Contact list display (Treeview)
contact_tree = ttk.Treeview(root, columns=("Name", "Phone"), show="headings", height=10)
contact_tree.heading("Name", text="Name")
contact_tree.heading("Phone", text="Phone")
contact_tree.pack(pady=20)

# Bind the selection event to populate fields
contact_tree.bind("<<TreeviewSelect>>", on_select)

# Start the main event loop
root.mainloop()

