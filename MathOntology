from owlready2 import *
import tkinter as tk
from tkinter import messagebox
import random

class MathITS:
    def __init__(self, root):
        self.root = root
        self.root.title("Ontology-Based Math ITS")
        self.root.configure(bg='black')

        # Create the welcome screen
        self.create_welcome_screen()

    def create_welcome_screen(self):
        # Clear existing widgets
        for widget in self.root.winfo_children():
            widget.destroy()

        # Welcome Label
        welcome_label = tk.Label(
            self.root, text="Welcome John Doe!", font=('Arial', 24), bg='black', fg='white'
        )
        welcome_label.pack(pady=20)

        # Instruction Label
        instruction_label = tk.Label(
            self.root, text="Please enter your password to advanced into your learning environment ""(Password for Testing the system is - password123):", font=('Arial', 14), bg='black', fg='white'
        )
        instruction_label.pack(pady=10)

        # Password Entry
        self.password_entry = tk.Entry(self.root, font=('Arial', 14), show="*", width=20)
        self.password_entry.pack(pady=10)

        # Submit Button
        submit_button = tk.Button(
            self.root, text="Submit", font=('Arial', 14), bg='green', fg='white', command=self.verify_password
        )
        submit_button.pack(pady=10)

    def verify_password(self):
        # Check the entered password
        password = self.password_entry.get()
        correct_password = "password123"  # Replace with your desired password

        if password == correct_password:
            self.create_learning_environment()
        else:
            messagebox.showerror("Error", "Incorrect password. Please try again.")

    def create_learning_environment(self):
        # Clear existing widgets
        for widget in self.root.winfo_children():
            widget.destroy()

        # Load the ontology
        self.onto = get_ontology("MathematicsITS").load()

        # Question-related attributes
        self.current_hint = None  # To store the current hint
        self.correct_answer = None  # To store the correct answer
        self.current_explanation = None  # To store the explanation
        self.score = 0  # To track the score

        # Score Label (Top-right corner)
        self.score_label = tk.Label(
            self.root, text=f"Score: {self.score}/5", font=('Arial', 14), bg='black', fg='white'
        )
        self.score_label.place(relx=0.95, rely=0.05, anchor='ne')

        # Question Labels
        self.question_label = tk.Label(
            self.root, text="Question will appear here.", font=('Arial', 14), bg='black', fg='white'
        )
        self.question_label.pack(pady=20)

        # Hint Label
        self.hint_label = tk.Label(self.root, text="", font=('Arial', 12), bg='black', fg='yellow')
        self.hint_label.pack(pady=10)

        # Explanation Label
        self.explanation_label = tk.Label(self.root, text="", font=('Arial', 12), bg='black', fg='cyan')
        self.explanation_label.pack(pady=10)

        # Answer Entry
        self.answer_entry = tk.Entry(self.root, font=('Arial', 14))
        self.answer_entry.pack(pady=10)

        # Submit Button with color
        self.submit_button = tk.Button(
            self.root, text="Submit", font=('Arial', 14), bg='green', fg='white', command=self.check_answer
        )
        self.submit_button.pack(pady=10)

        # Hint Button
        self.hint_button = tk.Button(
            self.root, text="Show Hint", font=('Arial', 14), bg='orange', fg='white', command=self.show_hint
        )
        self.hint_button.pack(pady=10)

        # Explanation Button (Initially hidden)
        self.explanation_button = tk.Button(
            self.root, text="Show Explanation", font=('Arial', 14), bg='purple', fg='white', command=self.show_explanation
        )
        self.explanation_button.pack(pady=10)
        self.explanation_button.config(state='disabled')  # Disable it initially

        # Feedback Label
        self.feedback_label = tk.Label(
            self.root, text="", font=('Arial', 14), bg='black', fg='white'
        )
        self.feedback_label.pack(pady=10)

        # Next Question Button with color
        self.next_button = tk.Button(
            self.root, text="Next Question", font=('Arial', 14), bg='blue', fg='white', command=self.next_question
        )
        self.next_button.pack(pady=10)
        self.next_button.config(state='disabled')  # Disable until answer is submitted

        self.generate_question()

    def generate_question(self):
        # Pick a random operation instance from all available operations
        operation_class = random.choice([
            self.onto.Addition,
            self.onto.Subtraction,
            self.onto.Multiplication,
            self.onto.Division,
        ])
        operation_instance = random.choice(list(operation_class.instances()))

        print(f"Selected Operation Instance: {operation_instance}")

        # Get operands and the correct answer
        try:
            operands_str = operation_instance.hasOperand[0]  # The operand is a string
            operands_str = operands_str.strip('"').strip()  # Remove quotes and extra spaces
            self.question_label.config(text=f"What is {operands_str}?")
            self.correct_answer = operation_instance.hasSolution[0].strip()  # The correct answer as a string

            # Get the hint associated with the operation
            hint_instance = (
                operation_instance.hasHint[0] if hasattr(operation_instance, 'hasHint') else None
            )
            if hint_instance:
                self.current_hint = hint_instance.hasHintText[0]  # Get the hint text
            else:
                self.current_hint = "No hint available for this question."

            # Get the explanation associated with the operation
            explanation_instance = (
                operation_instance.hasExplanation[0] if hasattr(operation_instance, 'hasExplanation') else None
            )
            if explanation_instance:
                self.current_explanation = explanation_instance.hasExplanationText[0]  # Get the explanation text
            else:
                self.current_explanation = "No explanation available for this question."

        except Exception as e:
            print(f"Error during question generation: {e}")
            self.question_label.config(text="Error generating the question. Please try again.")
            self.correct_answer = None
            self.current_hint = None
            self.current_explanation = None

        # Clear previous hint, explanation, and feedback
        self.hint_label.config(text="")
        self.explanation_label.config(text="")
        self.feedback_label.config(text="")

    def show_hint(self):
        # Display the current hint if available
        if self.current_hint:
            self.hint_label.config(text=f"Hint: {self.current_hint}")
        else:
            self.hint_label.config(text="No hint available for this question.")

    def show_explanation(self):
        # Display the current explanation if available
        if self.current_explanation:
            self.explanation_label.config(text=f"Explanation: {self.current_explanation}")
        else:
            self.explanation_label.config(text="No explanation available for this question.")

    def check_answer(self):
        # Get the user input as a string
        user_answer = self.answer_entry.get().strip()
        if user_answer == self.correct_answer:
            self.feedback_label.config(text="Correct! Well done.", fg="green")
            self.score += 1  # Increment the score on correct answer
            self.score_label.config(text=f"Score: {self.score}/5")  # Update the score display
            self.next_button.config(state='normal')  # Enable the Next button after correct answer
            self.submit_button.config(state='disabled')  # Disable the submit button
            self.explanation_button.config(state='normal')  # Enable the Explanation button after correct answer
        else:
            self.feedback_label.config(text="Incorrect, please try again.", fg="red")

        # Check if the score reached 5/5 and show the end screen
        if self.score == 5:
            self.show_end_screen()

    def next_question(self):
        self.generate_question()
        self.answer_entry.delete(0, tk.END)  # Clear the entry field
        self.feedback_label.config(text="")  # Clear previous feedback
        self.hint_label.config(text="")  # Clear previous hint
        self.explanation_label.config(text="")  # Clear previous explanation
        self.next_button.config(state='disabled')  # Disable until answer is submitted
        self.submit_button.config(state='normal')  # Re-enable the submit button
        self.explanation_button.config(state='disabled')  # Hide the Explanation button

    def show_end_screen(self):
        # Display the end screen with final score and remark
        self.question_label.config(text="You've completed the quiz!")
        self.feedback_label.config(text=f"Final Score: {self.score}/5", fg="cyan")

        if self.score == 5:
            remark = "Excellent! Perfect score!"
        else:
            remark = "Good job! Keep practicing."

        self.explanation_label.config(text=remark)  # Show the remark
        self.submit_button.config(state='disabled')  # Disable the submit button
        self.next_button.config(state='disabled')  # Disable the Next button
        self.explanation_button.config(state='disabled')  # Disable the Explanation button
        self.hint_button.config(state='disabled')  # Disable the Hint button

# Set up the main window
root = tk.Tk()
app = MathITS(root)
root.mainloop()
