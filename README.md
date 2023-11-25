my-books-website/
├── static/
│   └── style.css
├── templates/
│   └── index.html
├── app.py
└── books.db
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
    <title>Books on Sale</title>
</head>
<body>
    <h1>Books on Sale</h1>
    <div id="books-list"></div>
    <script src="https://code.jquery.com/jquery-3.6.4.min.js"></script>
    <script src="{{ url_for('static', filename='app.js') }}"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    margin: 20px;
}

h1 {
    text-align: center;
}

#books-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
}

.book {
    background-color: #fff;
    border: 1px solid #ddd;
    border-radius: 5px;
    margin: 10px;
    padding: 15px;
    width: 300px;
    text-align: center;
}

img {
    max-width: 100%;
    height: auto;
    border-radius: 5px;
    margin-bottom: 10px;
}
$(document).ready(function () {
    $.get("/books", function (data) {
        data.forEach(function (book) {
            var bookDiv = $("<div>").addClass("book");
            $("<img>").attr("src", book.image).appendTo(bookDiv);
            $("<h2>").text(book.title).appendTo(bookDiv);
            $("<p>").text("Author: " + book.author).appendTo(bookDiv);
            $("<p>").text("Price: $" + book.price).appendTo(bookDiv);
            $("<button>").text("Add to Cart").appendTo(bookDiv);
            $("#books-list").append(bookDiv);
        });
    });
});
from flask import Flask, render_template, jsonify
import sqlite3

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/books')
def get_books():
    conn = sqlite3.connect('books.db')
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM books')
    books = [{'title': title, 'author': author, 'price': price, 'image': image} for title, author, price, image in cursor.fetchall()]
    conn.close()
    return jsonify(books)

if __name__ == '__main__':
    app.run(debug=True)
CREATE TABLE books (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    author TEXT NOT NULL,
    price REAL NOT NULL,
    image TEXT NOT NULL
);

INSERT INTO books (title, author, price, image) VALUES
('Book 1', 'Author 1', 19.99, 'book1.jpg'),
('Book 2', 'Author 2', 24.99, 'book2.jpg');
pip install Flask
python app.py
