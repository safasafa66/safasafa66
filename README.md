import tkinter as tk
from tkinter import messagebox
from math import sqrt

class Calculator:
    def __init__(self, root):
        self.root = root
        self.root.title("آلة حاسبة")
        self.root.configure(bg='#8A2BE2')  # اللون البنفسجي للخلفية
        self.root.geometry("400x600")
        
        # خلفية مع قلوب بيضاء
        self.canvas = tk.Canvas(root, bg='#8A2BE2', height=600, width=400)
        self.canvas.pack(fill="both", expand=True)
        self.canvas.create_text(200, 100, text="❤", font=("Arial", 50), fill="white")
        self.canvas.create_text(100, 300, text="❤", font=("Arial", 50), fill="white")
        self.canvas.create_text(300, 500, text="❤", font=("Arial", 50), fill="white")
        
        # شاشة الإدخال
        self.display = tk.Entry(root, font=("Arial", 24), bd=10, insertwidth=2, width=14, borderwidth=4)
        self.display.place(x=50, y=20)
        
        # الأزرار
        self.create_buttons()
        
    def create_buttons(self):
        button_texts = [
            '7', '8', '9', '/',
            '4', '5', '6', '*',
            '1', '2', '3', '-',
            '0', '.', '=', '+',
            '(', ')', '√', '^',
            'C', 'DEL'
        ]
        
        row = 0
        col = 0
        for text in button_texts:
            self.create_button(text, row, col)
            col += 1
            if col > 3:
                col = 0
                row += 1
    
    def create_button(self, text, row, col):
        btn = tk.Button(self.root, text=text, padx=20, pady=20, font=("Arial", 18), 
                        command=lambda: self.on_button_click(text))
        btn.place(x=50 + col * 90, y=100 + row * 90)
        
    def on_button_click(self, char):
        if char == "=":
            self.calculate_result()
        elif char == "C":
            self.display.delete(0, tk.END)
        elif char == "DEL":
            current_text = self.display.get()
            self.display.delete(0, tk.END)
            self.display.insert(0, current_text[:-1])
        elif char == "√":
            current_text = self.display.get()
            try:
                result = sqrt(float(current_text))
                self.display.delete(0, tk.END)
                self.display.insert(0, str(result))
            except:
                messagebox.showerror("خطأ", "مدخل غير صالح")
        elif char == "^":
            current_text = self.display.get()
            self.display.insert(tk.END, "**")
        else:
            self.display.insert(tk.END, char)
    
    def calculate_result(self):
        try:
            result = eval(self.display.get())
            self.display.delete(0, tk.END)
            self.display.insert(0, str(result))
        except:
            messagebox.showerror("خطأ", "مدخل غير صالح")

if __name__ == "__main__":
    root = tk.Tk()
    calc = Calculator(root)
    root.mainloop()
