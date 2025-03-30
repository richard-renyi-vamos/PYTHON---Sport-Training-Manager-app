CHATGPT CONVERSATION


***************


create a SPORT TRAINING TIME & SCHEDULE manager app in PYTHON


ChatGPT said:
Here's a Python-based sport training time & schedule manager app with a simple GUI using Tkinter. It allows users to add, view, edit, and delete training schedules.

Sport Training Manager

Answer in chat instead
This app lets users manage their sports training schedule by adding, editing, and deleting sessions. It saves data in a JSON file for persistence. Let me know if you need enhancements, like a calendar view or export options! 🏋️‍♂️📅⚽

***************

import tkinter as tk
from tkinter import messagebox, simpledialog
import json

class TrainingScheduleApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Sport Training Schedule Manager")
        self.trainings = self.load_data()

        self.listbox = tk.Listbox(root, width=50, height=15)
        self.listbox.pack(pady=10)
        self.update_listbox()

        self.add_button = tk.Button(root, text="Add Training", command=self.add_training)
        self.add_button.pack()
        
        self.edit_button = tk.Button(root, text="Edit Training", command=self.edit_training)
        self.edit_button.pack()
        
        self.delete_button = tk.Button(root, text="Delete Training", command=self.delete_training)
        self.delete_button.pack()

    def load_data(self):
        try:
            with open("trainings.json", "r") as file:
                return json.load(file)
        except FileNotFoundError:
            return []

    def save_data(self):
        with open("trainings.json", "w") as file:
            json.dump(self.trainings, file, indent=4)

    def update_listbox(self):
        self.listbox.delete(0, tk.END)
        for training in self.trainings:
            self.listbox.insert(tk.END, f"{training['date']} - {training['activity']} - {training['duration']} mins")

    def add_training(self):
        date = simpledialog.askstring("Input", "Enter training date (YYYY-MM-DD):")
        activity = simpledialog.askstring("Input", "Enter training activity:")
        duration = simpledialog.askinteger("Input", "Enter training duration (minutes):")

        if date and activity and duration:
            self.trainings.append({"date": date, "activity": activity, "duration": duration})
            self.save_data()
            self.update_listbox()
        else:
            messagebox.showwarning("Warning", "All fields must be filled!")

    def edit_training(self):
        selected_index = self.listbox.curselection()
        if not selected_index:
            messagebox.showwarning("Warning", "Select a training to edit!")
            return
        index = selected_index[0]
        training = self.trainings[index]

        new_date = simpledialog.askstring("Edit", "Enter new date:", initialvalue=training['date'])
        new_activity = simpledialog.askstring("Edit", "Enter new activity:", initialvalue=training['activity'])
        new_duration = simpledialog.askinteger("Edit", "Enter new duration:", initialvalue=training['duration'])

        if new_date and new_activity and new_duration:
            self.trainings[index] = {"date": new_date, "activity": new_activity, "duration": new_duration}
            self.save_data()
            self.update_listbox()
        else:
            messagebox.showwarning("Warning", "All fields must be filled!")

    def delete_training(self):
        selected_index = self.listbox.curselection()
        if not selected_index:
            messagebox.showwarning("Warning", "Select a training to delete!")
            return
        index = selected_index[0]
        del self.trainings[index]
        self.save_data()
        self.update_listbox()

if __name__ == "__main__":
    root = tk.Tk()
    app = TrainingScheduleApp(root)
    root.mainloop()
