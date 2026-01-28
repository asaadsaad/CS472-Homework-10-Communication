## CS472 – Homework 10: Component Communication with React Context

### Book Library Management using React Context and CRUD Operations

In this assignment, you will build a React application that manages a book library using **React Context** for global state management and **CRUD operations** for data persistence. 
  
## Application Overview
Create a React application that interacts with a REST API to manage a list of books. The application must support **Create, Read, Update, and Delete (CRUD)** operations and share state across components using React Context.

You may use the following **mock API service**: https://67d17ef590e0670699ba5929.mockapi.io/books

### API Data Model

```ts
{
  id: number;
  title: string;
  author: string;
}
```
### Required CRUD Operations
Your application must implement the following API interactions:
* `GET`: Fetch all books when the application loads.
* `POST`: Add a new book.
* `PUT`: Update an existing book.
* `DELETE`: Remove a book.
All API calls should be handled inside the Context provider.
  
### React Context Setup
Create a BookContext that provides global access to:
* The list of books
* Functions to: add a book, update a book, delete a book
* Loading state (for API calls)
* Error state (for failed requests)
The context should act as the single source of truth for book data and business logic.

### Component Requirements
#### 1. BookProvider
* Wrap the entire application with `BookProvider`
* Owns the global state and API logic
* Uses `useEffect` to fetch books on initial mount

#### 2. AddBookForm
* A controlled form with inputs for: `title`, `author`
* All fields are required (basic validation)
* On submit: Calls addBook from context and clears the form on success

#### 3. BookList
* Displays all books from context
* Uses a grid or card layout
* Each book must render using a reusable `BookCard` component

#### 4. BookCard
* Displays: book `title`, `author`
* Includes: Edit button and Delete button
* Delete button calls `deleteBook` from context

#### 5. EditBookForm
* Shown when the user clicks Edit
* Pre-fills inputs with the selected book’s current data
* Uses controlled inputs
* On submit: Calls updateBook from context and returns the user to the book list view

### Side Effects and Data Flow
* Use useEffect inside the BookProvider to fetch initial data.
* All components must consume data and actions via useContext.
* Do not perform API calls directly inside UI components.
* Maintain one-way data flow:
   * Context → Components
   * Events → Context functions → State updates

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
  addBook: (book: Omit<Book, 'id'>) => Promise<Book>;
  updateBook: (id: string, updatedBook: Omit<Book, 'id'>) => Promise<Book>;
  deleteBook: (id: string) => Promise<Book>;
  loading: boolean;
  error: string | null;
}

const BookContext = createContext<BookContextType | null>(null);

export const BookProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [books, setBooks] = useState<Book[]>([]);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  // TODO:
  // - Fetch books with useEffect
  // - Implement addBook, updateBook, deleteBook

  return (
    <BookContext.Provider value={{ books, addBook, updateBook, deleteBook, loading, error }}>
      {children}
    </BookContext.Provider>
  );
};

export const useBookContext = () => {
  const ctx = useContext(BookContext);
  if (!ctx) {
    throw new Error("useBookContext must be used inside BookProvider");
  }
  return ctx;
};

```
### Optional Requirements
* Add a loading spinner during API calls
* Display user-friendly error messages
* Confirm before deleting a book
