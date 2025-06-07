import tkinter as tk

class Calculator(tk.Tk):
    def __init__(self):
        super().__init__()

        self.title("ماشین حساب")
        self.geometry("300x400")
        self.resizable(False, False)

        self.expression = ""

        self.display = tk.Entry(self, font=("Arial", 24), borderwidth=2, relief="ridge", justify="right")
        self.display.grid(row=0, column=0, columnspan=4, sticky="nsew", padx=10, pady=10)

        buttons = [
            ('C', 1, 0), ('⌫', 1, 1), ('%', 1, 2), ('/', 1, 3),
            ('7', 2, 0), ('8', 2, 1), ('9', 2, 2), ('*', 2, 3),
            ('4', 3, 0), ('5', 3, 1), ('6', 3, 2), ('-', 3, 3),
            ('1', 4, 0), ('2', 4, 1), ('3', 4, 2), ('+', 4, 3),
            ('±', 5, 0), ('0', 5, 1), ('.', 5, 2), ('=', 5, 3),
        ]

        for (text, row, col) in buttons:
            action = lambda x=text: self.on_button_click(x)
            tk.Button(self, text=text, width=5, height=2, font=("Arial", 18), command=action).grid(row=row, column=col, sticky="nsew", padx=2, pady=2)

        for i in range(6):
            self.grid_rowconfigure(i, weight=1)
        for j in range(4):
            self.grid_columnconfigure(j, weight=1)

    def on_button_click(self, char):
        if char == 'C':
            self.expression = ""
            self.update_display()
        elif char == '⌫':
            self.expression = self.expression[:-1]
            self.update_display()
        elif char == '=':
            try:
                # Replace % with /100 for percentage calculations
                expr = self.expression.replace('%', '/100')
                result = eval(expr)
                self.expression = str(result)
                self.update_display()
            except Exception:
                self.expression = ""
                self.display.delete(0, tk.END)
                self.display.insert(0, "Error")
        elif char == '±':
            if self.expression:
                if self.expression[0] == '-':
                    self.expression = self.expression[1:]
                else:
                    self.expression = '-' + self.expression
                self.update_display()
        else:
            self.expression += char
            self.update_display()

    def update_display(self):
        self.display.delete(0, tk.END)
        self.display.insert(0, self.expression)

if __name__ == "__main__":
    app = Calculator()
    app.mainloop()
