import json
from pathlib import Path

FILE = Path("books.json")

def load_books():
    if FILE.exists():
        return json.loads(FILE.read_text(encoding="utf-8"))
    return []

def save_books(books):
    FILE.write_text(
        json.dumps(books, indent=4, ensure_ascii=False),
        encoding="utf-8"
    )

def add_book(books):
    title = input("Book title: ").strip()
    author = input("Author: ").strip()

    try:
        pages = int(input("Total pages: "))

        if not title or pages <= 0:
            print("Invalid book details.")
            return

        books.append({
            "title": title,
            "author": author,
            "pages": pages,
            "read": 0
        })

        save_books(books)
        print("Book added successfully!")

    except ValueError:
        print("Enter a valid page count.")

def show_books(books):
    if not books:
        print("No books found.")
        return

    for index, book in enumerate(books, 1):
        progress = book["read"] / book["pages"] * 100

        print(f"\n{index}. {book['title']}")
        print(f"Author: {book['author']}")
        print(f"Progress: {progress:.1f}%")
        print(f"Pages: {book['read']}/{book['pages']}")

def update_progress(books):
    show_books(books)

    try:
        index = int(input("\nBook number: ")) - 1
        book = books[index]

        pages = int(input("Total pages read: "))

        if not 0 <= pages <= book["pages"]:
            print("Invalid page number.")
            return

        book["read"] = pages
        save_books(books)
        print("Progress updated!")

    except (ValueError, IndexError):
        print("Invalid input.")

def main():
    books = load_books()

    while True:
        print("\n1. Add book")
        print("2. Show books")
        print("3. Update progress")
        print("4. Exit")

        choice = input("Choose: ")

        if choice == "1":
            add_book(books)
        elif choice == "2":
            show_books(books)
        elif choice == "3":
            update_progress(books)
        elif choice == "4":
            break
        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
