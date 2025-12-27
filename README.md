import json
import os

FILE_NAME = "students.json"

# Load data from JSON file
def load_data():
    if not os.path.exists(FILE_NAME):
        return []
    with open(FILE_NAME, "r") as file:
        return json.load(file)

# Save data to JSON file
def save_data(data):
    with open(FILE_NAME, "w") as file:
        json.dump(data, file, indent=4)

# Validate integer input
def get_int_input(message):
    while True:
        try:
            return int(input(message))
        except ValueError:
            print("❌ Please enter a valid number.")

# Add student
def add_student():
    students = load_data()
    roll = get_int_input("Enter Roll Number: ")

    for s in students:
        if s["roll"] == roll:
            print("❌ Student already exists.")
            return

    name = input("Enter Name: ").strip()
    age = get_int_input("Enter Age: ")
    course = input("Enter Course: ").strip()

    students.append({
        "roll": roll,
        "name": name,
        "age": age,
        "course": course
    })

    save_data(students)
    print("✅ Student added successfully.")

# View students in tabular format
def view_students():
    students = load_data()
    if not students:
        print("❌ No records found.")
        return

    print("\n{:<10} {:<20} {:<10} {:<15}".format("ROLL", "NAME", "AGE", "COURSE"))
    print("-" * 55)
    for s in students:
        print("{:<10} {:<20} {:<10} {:<15}".format(
            s["roll"], s["name"], s["age"], s["course"]
        ))

# Update student
def update_student():
    students = load_data()
    roll = get_int_input("Enter Roll Number to update: ")

    for s in students:
        if s["roll"] == roll:
            s["name"] = input("Enter New Name: ")
            s["age"] = get_int_input("Enter New Age: ")
            s["course"] = input("Enter New Course: ")
            save_data(students)
            print("✅ Student updated successfully.")
            return

    print("❌ Student not found.")

# Delete student
def delete_student():
    students = load_data()
    roll = get_int_input("Enter Roll Number to delete: ")

    for s in students:
        if s["roll"] == roll:
            students.remove(s)
            save_data(students)
            print("✅ Student deleted successfully.")
            return

    print("❌ Student not found.")

# Main menu
def menu():
    while True:
        print("\n===== STUDENT RECORD SYSTEM =====")
        print("1. Add Student")
        print("2. View Students")
        print("3. Update Student")
        print("4. Delete Student")
        print("5. Exit")

        choice = input("Enter choice: ")

        if choice == "1":
            add_student()
        elif choice == "2":
            view_students()
        elif choice == "3":
            update_student()
        elif choice == "4":
            delete_student()
        elif choice == "5":
            print("👋 Exiting program.")
            break
        else:
            print("❌ Invalid choice.")

# Run program
menu()
