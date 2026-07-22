
##  Project: Library Book Management API

Build a RESTful API for a library system that handles books and their user reviews. This project covers core REST principles, standard HTTP verbs, status codes, nested resources, filtering, and pagination.

---
## 🛠️ Required Endpoints & REST Guidelines

### 1. Books Management (`/api/books`)

* **List Books**: `GET /api/books`
* Should return a `200 OK` with a list of books.
* **Support Query Parameters**:
* Filtering by genre: `?genre=Fiction`
* Search by title: `?search=clean`
* Pagination: `?page=1&pageSize=10`




* **Get Single Book**: `GET /api/books/{id}`
* Return `200 OK` with the book details.
* Return `404 Not Found` with a standard error body if the ID doesn't exist.


* **Create Book**: `POST /api/books`
* Accepts book details in the request body (e.g., `Title`, `Author`, `Genre`, `PublishYear`).
* Validate inputs (e.g., `Title` and `Author` are required). Return `400 Bad Request` on invalid data.
* Return `201 Created` with a `Location` header pointing to `GET /api/books/{id}`.


* **Update Book (Full)**: `PUT /api/books/{id}`
* Replaces the book entity entirely. Return `204 No Content` on success or `404 Not Found`.


* **Update Book (Partial)**: `PATCH /api/books/{id}`
* Partially updates fields (e.g., updating just the `Genre`).


* **Delete Book**: `DELETE /api/books/{id}`
* Remove the book. Return `204 No Content` on success or `404 Not Found`.



---

### 2. Nested Resource: Book Reviews (`/api/books/{bookId}/reviews`)

* **List Reviews for a Book**: `GET /api/books/{bookId}/reviews`
* Returns all reviews belonging to a specific book.


* **Add a Review**: `POST /api/books/{bookId}/reviews`
* Accepts `Rating` (1–5) and `Comment`.
* Return `201 Created` with the newly created review object.



---

## 📋 Recommended Data Models

```csharp
public class Book
{
    public int Id { get; set; }
    public string Title { get; set; } = string.Empty;
    public string Author { get; set; } = string.Empty;
    public string Genre { get; set; } = string.Empty;
    public int PublishYear { get; set; }
    public List<Review> Reviews { get; set; } = new();
}

public class Review
{
    public int Id { get; set; }
    public int BookId { get; set; }
    public int Rating { get; set; } // 1 to 5
    public string Comment { get; set; } = string.Empty;
    public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
}

```

---

##  Bonus Engineering Challenges

1. **Global Exception Handling**: Implement middleware or a `ProblemDetails` response handler so all unexpected errors return a consistent error structure:
```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  "title": "Not Found",
  "status": 404,
  "detail": "Book with ID 42 was not found."
}

```


2. **DTOs & AutoMapper/Mapster**: Avoid exposing your domain entities directly in the API responses. Use Data Transfer Objects (DTOs) for incoming payloads and responses.
3. **In-Memory database or EF Core SQLite**: Keep setup simple by using Entity Framework Core with `UseInMemoryDatabase()` or SQLite.