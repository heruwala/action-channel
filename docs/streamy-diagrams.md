## Domain Model
```mermaid
---
title: Streamy Domain Model
---
classDiagram
  Title "1..*" -- "1..*" Genre: is associated with

  Title "1" * -- "0..*" Season: has
  Title "1" * -- "0..*" Review: has
  Title "0..*" o-- "1..*" Actor: features

  TV Show --|> Title: implements
  Short --|> Title: implements
  Film --|> Title: implements

  Viewer "0..*" --> "0..*" Title: watches

  Season "1" *-- "0..*" Review: has
  Season "1" *-- "1..*" Episode: contains

  Episode "1" *-- "0..*" Review: has
```
## Sequence Diagram
```mermaid
---
title: User Sign Up Flow
---
sequenceDiagram
autonumber
  actor Browser
  participant Sign Up Service
  participant User Service
  participant Kafka

  links User Service: {"Repository": "https://www.example.com/repository"}

  Browser->>Sign Up Service: GET /sign_up
  activate Sign Up Service

  Sign Up Service-->>Browser: 200 OK (HTML page)
  deactivate Sign Up Service
  Browser->>+Sign Up Service: POST /sign_up
  Sign Up Service->>Sign Up Service: Validate input

  alt invalid input
      Sign Up Service-->>Browser: Error
  else valid input
      Sign Up Service->>+User Service: POST /users
      User Service--)Kafka: User Created Event Published
      Note left of Kafka: other services take action based on this event
      User Service-->>-Sign Up Service: 201 Created (User)
      Sign Up Service-->>-Browser: 301 Redirect (Login page)
  end
```
## Flowchart Container Diagram with C4 Model and subgraphs  
<sub>container diagram details each deployable unit in your architecture</sub>
```mermaid
---
title: "Listing Service C4 Model: System Context"
---
flowchart TD
  User["Premium Member
  [Person]
  A user of the website who has
  purchased a subscription"]

  WA["Web Application
  [.NET Core MVC Application]
  Allows members to view and review titles 
  from a web browser. Also exposes
  an API for the mobile app"]

  MA["Mobile Application
  [Xamarin Application]
  Allows members to view and review
  titles from their mobile devices"]

  R[("In-Memory Cache
  [Redis]
  Titles and their reviews
  are cached")]

  K["Message Broker
  [Kafka]

  Important domain events
  are published to Kafka"]

  TS["Title Service
  [Software System]

  Provides an API to retrieve
  title information"]

  RS["Reviews Service
  [Software System]

  Provides an API to retrieve
  and submit reviews"]

  SS["Search Service
  [Software System]

  Provides an API to search
  for titles"]

  User-- "Views titles, searches titles
  and reviews titles using
  [HTTPS]" -->WA

	User-- "Views titles, searches titles
	and reviews titles using
	[HTTPS]" -->MA

	subgraph listing-service[Listing Service]
	  MA-- "Makes API calls to\n[HTTPS]" -->WA

    WA-- "Reads and writes to\n[Redis Serialization Protocol]" -->R
  end

  WA-. "Publishes messages to\n[Binary over TCP]" ..->K
  WA-- "Makes API calls to\n[HTTPS]" --->TS
  WA-- "Makes API calls to\n[HTTPS]" --->RS
  WA-- "Makes API calls to\n[HTTPS]" --->SS

  classDef container fill:#1168bd,stroke:#0b4884,color:#ffffff
  classDef person fill:#08427b,stroke:#052e56,color:#ffffff
  classDef supportingSystem fill:#666,stroke:#0b4884,color:#ffffff
  class User person
  class WA,MA,R container
  class TS,RS,SS,K supportingSystem
  style listing-service fill:none,stroke:#CCC,stroke-width:2px
  style listing-service color:#fff,stroke-dasharray: 5 5
```
### Additional Arrow Types
```mermaid
---
title: "Arrow Types"
---
flowchart LR
  A[Normal Arrow] --> B[Dashed Arrow]
  B-.-> C[Thick Arrow]
  C==> D[Dotted with text]
  D-.text.-> E[Thick link with text]
  E==text==> F[END]

  G --> H & I --> J
```
## Component Diagram
```mermaid
---
title: "Listing Service C4 Model: Component Diagram"
---
flowchart TD
    classDef container fill:#1168bd,stroke:#0b4884,color:#ffffff
    classDef externalSystem fill:#666,stroke:#0b4884,color:#ffffff
    classDef component fill:#85bbf0,stroke:#5d82a8,color:#000000

    Browser["Browser
    [Web Browser]

    Used by a user to browse
    the website"]

    MA["Application
    [Xamarin Application]

    Allows members to view and review
    titles from their mobile devices"]

    R["In-Memory Cache
    [Redis]

    Titles and their reviews
    are cached"]

    K["Message Broker
    [Kafka]

    Important domain events
    are published to Kafka"]

    TS["Title Service
    [Software System]

    Provides an API to retrieve
    title information"]
    RS["Review Service
    [Software System]

    Provides an API to retrieve
    and submit reviews"]

    SS["Search Service
    [Software System]

    Provides an API to search
    for titles"]

    TCont["Title Controller
    [ASP.NET MVC Controller]

    Allows users to view details
    about titles"]

    SCont["Search Controller
    [ASP.NET MVC Controller]

    Allows users to search
    for titles"]

    RCont["Review Controller
    [ASP.NET MVC Controller]

    Allows users to read and
    write reviews"]
    TComp["Title Component
    [ASP.NET Namespace]

    Provides information on titles,
    retrieves information from the title service
    and caches titles"]

    SComp["Search Component
    [ASP.NET Namespace]

    Searches titles using the
    search service"]

    RComp["Review Component
    [ASP.NET Namespace]

    Provides review information,
    submits new reviews
    and publishes domain events"]
    Browser-- "Submits requests to\n[HTTPS]" --->TCont
    MA-- "Submits requests to\n[HTTPS]" --->TCont

    MA-- "Submits requests to\n[HTTPS]" --->SCont
    Browser-- "Submits requests to\n[HTTPS]" --->SCont

    MA-- "Submits requests to\n[HTTPS]" --->RCont
    Browser-- "Submits requests to\n[HTTPS]" --->RCont

    subgraph listing-service[Listing Service]
	        TCont--->TComp
	        RCont--->TComp
	        RCont--->RComp

	        SCont--->SComp
	    end

	    TComp--->TS
	    TComp--->R
	RComp--->R
	RComp--->K
	RComp--->RS

	SComp--->SS

	class MA,R container
	class SS,RS,TS,K,Browser externalSystem
	class RComp,SComp,TComp,RCont,SCont,TCont component
	style listing-service fill:none,stroke:#CCC,stroke-width:2px
	style listing-service color:#fff,stroke-dasharray: 5 5
```
## Entity Relationship Diagram
Note:
- Zero, represented by an o.
- One, represented by a |.
- Many, represented by a {.
- o{ translates to zero (minimum) to many (maximum)

Relationships:
- -- identifying relationship. Entity cannot exist without the other.
- .. non-identifying relationship. Entity can exist independent of the other.
- one-to-many (|{)
- zero-to-many (o{)
- one-to-one (||) 
- zero-or-one (o|)

```mermaid
---
title: Streamy Entity Relationship Diagram
---
erDiagram
  TITLE {
    int title_id PK
    int type_id FK "This is comment"
    string name
    datetime release_date
  }

  TITLE_TYPE {
    int type_id PK
    string type
  }

  ACTOR {
    int actor_id PK
    string name
    date date_of_birth
  }

  TITLE_ACTOR {
      int title_id PK "FK"
      int actor_id PK "FK"
  }

  GENRE {
    int genre_id PK
    string name
  }

  TITLE_GENRE {
    int title_id PK "FK"
    int genre_id PK "FK"
  }

  EPISODE {
    int episode_id PK
    int season_id FK
    string name
    int season_number
    int episode_number
    datetime release_date
  }

  SEASON {
      int season_id PK
      int title_id FK
      int season_number
      date release_year
  }

  REVIEW {
      int review_id PK
      int title_id FK
      int season_id FK
      string review_by
      datetime review_date
      string review_text
 }

  TITLE }|..|| TITLE_TYPE : has
  TITLE ||--o{ TITLE_GENRE : "belongs to"
  TITLE ||--|{ TITLE_ACTOR: features
  TITLE ||..|{ SEASON : contains

  TITLE_GENRE }o--|| GENRE : references

  TITLE_ACTOR }|--|| ACTOR: references

  EPISODE }|..|| SEASON: contains

  REVIEW }o..o| TITLE: "made against"
  REVIEW }o..o| EPISODE: "made against"
  REVIEW }o..o| SEASON: "made against"
```

## Detailed Class Diagram
```mermaid
---
title: POST /users Request Handling
---
sequenceDiagram
    autonumber

    participant UserController
    participant CreateUserService
    participant UserModel
    participant SendWelcomeEmailService
    participant Kafka

    UserController->>+CreateUserService: call
    CreateUserService->>UserModel: find_users_by_email
    UserModel-->>CreateUserService: array of Users

    loop
        CreateUserService->>CreateUserService: check_active_users
    end

    CreateUserService->>UserModel: create_user
    UserModel-->>CreateUserService: User

    par
        CreateUserService->>SendWelcomeEmailService: send_welcome_email
        SendWelcomeEmailService-->>CreateUserService: boolean

        CreateUserService->>Kafka: publish_user_created_event
        Kafka-->>CreateUserService: boolean
    end

    CreateUserService-->>-UserController: User
```
## Class Diagram
```mermaid
classDiagram

   class CreateUserRequest {
        +string Email
        +string Username

        +Validate() bool
   }

   class UserController {
       <<Class>>
       -ICreateUserService _createUserService
       +UserController(ICreateUserService createUserService)
       +Create(string email, string username) User
   }

   class ICreateUserService {
       <<Interface>>
       +Call(CreateUserRequest createUserRequest) User
   }

   class CreateUserService {
       <<Class>>
       -UserModel _userModel
       -SendWelcomeEmailService _sendWelcomeEmailService
       -Kafka _kafka
       +CreateUserService(UserModel um, SendWelcomeEmailService es, Kafka k)
       +Call(CreateUserRequest createUserRequest) User
       -CheckActiveUsers(List~User~ users) bool
   }

    class UserModel {
        +FindUsersByEmail(string email) List~User~
        +CreateUser(string email, string username) User
    }

    class User {
        +int UserId
        +string Username
        +string Email
    }

    class SendWelcomeEmailService {
        +Call(User user) bool
    }

    class Kafka {
        +PublishUserCreatedEvent(User User) bool
    }

    UserController ..> ICreateUserService: depends on
    UserController ..|> CreateUserRequest: depends on
    CreateUserService ..|> ICreateUserService: implements
    CreateUserService ..> UserModel: depends on
    UserModel ..> User: depends on
    CreateUserService ..> SendWelcomeEmailService: depends on
    CreateUserService ..> Kafka: depends on
```