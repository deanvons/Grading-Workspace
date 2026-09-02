
# Music Label API

Create a Web API and document it.

## 1) Assignment Overview

You are tasked with creating a datastore and interface to store and manipulate data about music labels, their albums, and the artists who contribute to those albums. The application should be constructed in ASP.NET Core and include a database made in SQL Server through EF Core. You can use another tech, if necessary (Spring Boot etc). A RESTful API will allow users to manipulate the data.

## 2) Set up the development environment

Ensure you have the necessary tools installed:

## 3) Optional: Database diagrams

You can use SSMS or a similar tool to draw relationship diagrams of your database and include these in your repository.

---

## 4) EF Core Code First

### Introduction and Overview

Your application will include the following entities:
- **Music Labels**: Represents the music labels that produce albums.
- **Albums**: Represents the music albums, which belong to a single label.
- **Artists**: Represents the artists, who can contribute to multiple albums.

### Business Rules

- **Artists and Albums**:
  - One album can feature many artists, and an artist can contribute to multiple albums.
- **Albums and Music Labelps**:
  - One album belongs to one music label, but a music label can produce many albums.

### Data Requirements

#### Music Label
- Autoincremented Id
- Name
- Description

#### Album
- Autoincremented Id
- Album title
- Genre (a simple string of comma-separated genres, no genre modeling required)
- Release year
- LabelId (foreign key)
- CoverImage (URL to the album cover)

#### Artist
- Autoincremented Id
- Full name
- Stage name (if applicable)
- Picture (URL to a photo)
- Biography (short description)

**Note**: Decide nullability and length limitations based on context. Avoid NVARCHAR(MAX) for all strings.

### Seeding

Seed the database with at least:
- 3 Music Labels.
- A total of at least 6 Albums distributed among the labels.
- A minimum of 10 Artists.

---

## 5) Web API

### Introduction and Overview

Create a Web API with the following minimum requirements.

### API Requirements

#### Generic CRUD

Provide full CRUD functionality for:
- Music Labels.
- Albums.
- Artists.

Ensure that:
- Proper deletes are performed (related data is not deleted; foreign keys are set to null).

#### Updating Related Data

Create dedicated endpoints to:
- Update artists in an album (takes an integer array of artist IDs in the body and an Album ID in the path).
- Update albums in a music label (takes an integer array of album IDs in the body and a Music Label ID in the path).

#### Reporting

Provide the following reports in addition to basic reads:
1. Get all albums produced by a specific music label.
2. Get all artists featured in a specific album.
3. Get all artists under a specific music label.

### DTOs with AutoMapper

Create DTOs for all model classes clients will interact with. Use AutoMapper or a similar tool to map between your domain models (entities) and these DTOs.

#### Notes:
- Create DTOs for adding new entities, updating existing ones, and retrieving data.
- Encapsulate DTO mapping in controllers (services return domain objects).

### Documentation with Swagger

Use Swagger/OpenAPI to document your API. Include:
- Summary (///) tags for each method explaining its functionality, possible exceptions, and return data.
- Swagger annotations for better documentation.

##	Services and/or Repositories
The use of Services/Repositories can really help clean up your controllers when there is a lot of DbContext manipulation that needs to be done. You should use at least Services, Repositories are optional. Ensure these Services/Repositories you make can be injected (registered in builder.Services).

---

## 6) Optional Advanced Requirements

1. **Search and Filter Feature**:
   Add an endpoint to allow users to search and filter albums by genre, release year, or label name.


---

## 7) Submission

Submit the following in a private GitLab repository:
- All your code, including a well-structured README file.
- Generated database diagrams (if created).
- Proper commit messages and branch organization.
- Add your lecturer as a Maintainer.


---

This assignment helps you build a full-stack database and API with real-world functionality. Good luck managing the music labels and their stars!


# Advanced Extensions for Music Label API (optional)

Here are additional advanced extensions to further enhance the functionality of the Music Label API:

---

## **1. Conditional Inclusion of Related Data**

### Description
Allow the inclusion of related entities in API responses based on a boolean query parameter or header.

### Implementation
- **Endpoint**: `GET /api/albums/{id}`
- **Feature**: If the client includes a boolean query parameter (`?includeArtists=true`) or a custom header (`X-Include-Artists: true`), the response should include the related artist objects for the album.

### Examples
**Response Without Header**:
```json
{
  "id": 1,
  "title": "Thriller",
  "genre": "Pop",
  "releaseYear": 1982
}
```

**Response With Header/Parameter**:
```json
{
  "id": 1,
  "title": "Thriller",
  "genre": "Pop",
  "releaseYear": 1982,
  "artists": [
    { "id": 101, "fullName": "Michael Jackson" }
  ]
}
```

---

## **2. Soft Delete for Artists and Albums**

### Description
Implement a "soft delete" mechanism instead of permanently removing artists or albums. Use a `Deleted` flag in the database to mark an entity as deleted. Exclude these entities from standard responses unless explicitly requested.

### Implementation
- Include a query parameter (`?includeDeleted=true`) to return all entities, including soft-deleted ones.
- Add an endpoint for restoring a deleted entity:
  - **Endpoint**: `POST /api/artists/{id}/restore`

---

## **3. Bulk Import of Data**

### Description
Allow bulk creation of music labels, albums, or artists through a single API call.

### Implementation
- **Endpoint**: `POST /api/albums/bulk`
- **Request Payload**:
```json
[
  { "title": "Album 1", "genre": "Rock", "releaseYear": 2000, "labelId": 1 },
  { "title": "Album 2", "genre": "Jazz", "releaseYear": 2010, "labelId": 2 }
]
```
- Validate each record before inserting it into the database.

---

## **4. Pagination and Sorting for Lists**

### Description
Add pagination and sorting functionality for endpoints returning lists of entities.

### Implementation
- Add support for query parameters `?page=1&limit=10&sort=name` to control the data returned.
- Include metadata in the response, such as total items and total pages.

### Example Response
```json
{
  "data": [
    { "id": 1, "title": "Album 1" },
    { "id": 2, "title": "Album 2" }
  ],
  "pagination": {
    "currentPage": 1,
    "totalPages": 5,
    "totalItems": 50
  }
}
```

---

These extensions add advanced functionality to the API, providing a richer, more versatile experience for developers and users.
