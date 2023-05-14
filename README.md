# SQLAlchemy-library

I want to keep track of the books I have read and give each book a rating.
I'm hooking up my database with a Flask application to serve data whenever needed.

writing SQL commands are complicated and error-prone. It would be much better if we could just write Python code and get the compiler to help us spot typos and errors in our code. That's why SQLAlchemy was created.
SQLAlchemy is defined as an ORM Object Relational Mapping library. This means that it's able to map the relationships in the database into Objects. Fields become Object properties. Tables can be defined as separate Classes and each row of data is a new Object. This will make more sense after we write some code and see how we can create a Database/Table/Row of data using SQLAlchemy.

I'm going to build an SQLite database into the Flask Website. So that any books added are stored in the database and I'm also going to build in some extra features to take advantage of the full CRUD features:

Create

Read

Update

Delete

# My notes

## Create a New Database
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = "sqlite:///<name of database>.db"
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
db = SQLAlchemy(app)

## Create a New Table
class Book(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(250), unique=True, nullable=False)
    author = db.Column(db.String(250), nullable=False)
    rating = db.Column(db.Float, nullable=False

app.app_context.push()  
db.create_all()
  
## Create A New Record
new_book = Book(id=1, title="Harry Potter", author="J. K. Rowling", rating=9.3)
db.session.add(new_book)
db.session.commit()
  
## Read All Records
all_books = db.session.query(Book).all()


## Read A Particular Record By Query
book = Book.query.filter_by(title="Harry Potter").first()


## Update A Particular Record By Query
book_to_update = Book.query.filter_by(title="Harry Potter").first()
book_to_update.title = "Harry Potter and the Chamber of Secrets"
db.session.commit()  


## Update A Record By PRIMARY KEY
book_id = 1
book_to_update = Book.query.get(book_id)
book_to_update.title = "Harry Potter and the Goblet of Fire"
db.session.commit()  


## Delete A Particular Record By PRIMARY KEY
book_id = 1
book_to_delete = Book.query.get(book_id)
db.session.delete(book_to_delete)
db.session.commit()  
  
