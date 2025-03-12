## CS472-Homework-10-Communication

### Book Library Management with React Context and CRUD Operations
Create a React application that interacts with the provided API to manage a book library. Implement full CRUD (Create, Read, Update, Delete) operations using React Context for state management.

You may use the following [mock API service](https://67d17ef590e0670699ba5929.mockapi.io/books) or create your own account. The service provides `/books` endpoint with the following fields
```typescript
{
  "id": number,
  "title": string,
  "author": string,
}
```
You will perform all CRUD operations:
* GET to fetch all books.
* POST to add a new book.
* PUT to update an existing book.
* DELETE to remove a book.
  
### React Context Setup
Create a `BookContext` providing: 
* The list of books.
* Functions for adding, updating, and deleting books.
* Loading and error states for API calls.

### Mount All Components
* AddBookForm: A controlled form with validation (all fields required). On submission, send a POST request.
* BookList: Displays all books in a grid or card layout. Each book card should show its details and have Edit and Delete buttons.
* EditBookForm: When Edit is clicked, pre-fills the book’s current data and sends a PUT request on submission.
* Use useEffect to fetch initial data when the component mounts.

### Example Context Structure
```typescript
// BookContext.tsx
import React, { createContext, useContext, useState, useEffect } from 'react';

interface Book {
  id: number;
  title: string;
  author: string;
}

interface BookContextType {
  books: Book[];
  addBook: (book: Omit<Book, 'id'>) => Promise<void>;
  updateBook: (id: string, updatedBook: Omit<Book, 'id'>) => Promise<void>;
  deleteBook: (id: string) => Promise<void>;
  loading: boolean;
  error: string | null;
}

const BookContext = createContext<BookContextType | null>(null);

export const BookProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [books, setBooks] = useState<Book[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  // Implement CRUD functions and useEffect here

  return (
    <BookContext.Provider value={{ books, addBook, updateBook, deleteBook, loading, error }}>
      {children}
    </BookContext.Provider>
  );
};

export const useBookContext = () => useContext(BookContext)!;
```
