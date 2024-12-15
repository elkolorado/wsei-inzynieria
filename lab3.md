# Pizza Finder 

# Opis projektu

Pizza Finder to aplikacja webowa umożliwiająca użytkownikom wyszukiwanie i porównywanie ofert pizzy z różnych restauracji. Głównym celem jest ułatwienie procesu wyboru pizzy poprzez zaawansowane filtrowanie i sortowanie wyników.

## Główne funkcje

- Wyszukiwanie pizzy po nazwie, składnikach i restauracji
- Filtrowanie wyników według:
  - Ceny
  - Czasu dostawy
  - Składników
  - Lokalizacji restauracji
- Porównywanie cen tej samej pizzy z różnych restauracji
- Zapisywanie ulubionych pizz i restauracji

## Korzyści dla użytkownika
- Oszczędność czasu przy wyborze pizzy
- Łatwe porównywanie cen
- Personalizacja wyników
- Szybki dostęp do ulubionych pozycji

## Grupa docelowa
- Studenci i młodzi profesjonaliści
- Rodziny zamawiające pizzę do domu
- Osoby poszukujące najlepszych ofert
- Miłośnicy pizzy szukający nowych smaków

# Wymagania funkcjonalne

1. Wyszukiwanie i filtrowanie
   - System umożliwia wyszukiwanie pizzy po nazwie
   - System pozwala na filtrowanie wyników po składnikach
   - System umożliwia sortowanie wyników po ilości skadników, cenie i czasie dostawy
   - System wyświetla szczegółowe informacje o każdej pizzy

2. Zarządzanie kontem użytkownika
   - Użytkownik może utworzyć konto
   - Użytkownik może się zalogować/wylogować
   - Użytkownik może edytować swój profil
   - Użytkownik może zarządzać swoimi preferencjami

3. Ulubione i historia
   - Użytkownik może dodawać pizze do ulubionych
   - System zapisuje ostatnie wyszukiwania


# Wymagania niefunkcjonalne

1. Wydajność
   - Czas odpowiedzi systemu poniżej 2 sekund
   - Obsługa minimum 1000 równoczesnych użytkowników
   - Dostępność systemu na poziomie 99.9%
   - Efektywne cachowanie często wyszukiwanych wyników

2. Bezpieczeństwo
   - Szyfrowanie danych użytkowników
   - Bezpieczne przechowywanie haseł
   - Ochrona przed atakami CSRF i XSS
   - Regularne kopie zapasowe danych

3. Użyteczność
   - Intuicyjny interfejs użytkownika
   - Responsywny design (mobile-first)
   - Wsparcie dla różnych przeglądarek
   - Czas ładowania strony poniżej 3 sekund
   - Wielojęzyczność

4. Skalowalność
   - Elastyczna architektura pozwalająca na rozbudowę
   - Możliwość obsługi wzrostu ruchu
   - Modułowa struktura kodu




# Specyfikacja techniczna

## Frontend
- **Framework**: React/Angular
- **State Management**: Redux/MobX 
- **UI Components**: Material-UI/Tailwind
- **Responsive Design**

## Backend
- **Framework**: Spring Boot/Node.js
- **API Gateway**: Spring Cloud Gateway/Express Gateway
- **Walidacja**: JSON Schema
- **Cache**: Redis
- **Database**: PostgreSQL

## Integracje
- REST API Pyszne.pl
- JWT Authentication
- Rate Limiting
- Circuit Breaker

## Funkcjonalności
- Wielokryterialne wyszukiwanie pizzy
- Filtrowanie po składnikach
- Sortowanie wyników (cena, ocena, czas dostawy)
- Cachowanie popularnych wyników
- Obsługa błędów API
- Monitoring i logowanie

## Bezpieczeństwo
- HTTPS
- API Key Management
- Input Sanitization
- Rate Limiting
- CORS Policy

## Monitoring
- Health Checks
- Performance Metrics
- Error Tracking
- User Analytics



# Architektura


```mermaid
flowchart TD
    subgraph Frontend
        UI[Interfejs użytkownika] --> LoginComponent[Komponent logowania]
        LoginComponent --> PizzaSelector[Komponent wyboru składników]
        PizzaSelector --> ResultsList[Lista wyników]
        ResultsList --> RestaurantDetails[Szczegóły restauracji]
        RestaurantDetails --> FavoritesComponent[Komponent ulubionych]
        FavoritesComponent --> SaveFavorite[Zapisz do ulubionych]
    end

    subgraph Backend Services
        APIGateway[API Gateway] --> PizzaService[Serwis Pizzy]
        APIGateway --> UserService[Serwis Użytkownika]
        APIGateway --> FavoritesService[Serwis Ulubionych]
        PizzaService --> RestaurantService[Serwis Restauracji]
        RestaurantService --> SearchService[Serwis Wyszukiwania]
        
        SearchService --> DataValidator[Walidator danych]
        DataValidator --> IntegrationService[Serwis Integracji]
    end

   subgraph Data Layer
   
               subgraph Cache Strategy
               Cache --> L1Cache[L1 - Aplikacyjny]
               Cache --> L2Cache[L2 - Distributed]
         end

      subgraph Integration Layer
         IntegrationService --> PyszneAdapter[Adapter Pyszne.pl]
         
         PyszneAdapter --> PyszneAPI[API Pyszne.pl]

      end

      subgraph Database Layer
         SearchService --> Cache[Redis Cache]
         SearchService --> Database[(Baza danych)]
         UserService --> Database
         FavoritesService --> Database
         

      end
    end

    subgraph Common
        AuthService[Serwis Autoryzacji]
        ErrorHandler[Obsługa błędów]
        Logger[System logowania]
        Monitoring[System monitoringu]
    end

    UI -->|HTTP/REST| APIGateway
    AuthService --> APIGateway
    ErrorHandler --> APIGateway
    Logger --> APIGateway
    Monitoring --> APIGateway
```
## Common

### AuthService (Serwis Autoryzacji)
- Odpowiada za uwierzytelnianie i autoryzację użytkowników
- Zarządza tokenami JWT
- Kontroluje dostęp do różnych części aplikacji

### ErrorHandler (Obsługa błędów)
- Centralne miejsce do obsługi wszystkich błędów w systemie
- Standaryzuje format odpowiedzi błędów
- Może zawierać logikę retry (ponownych prób) dla nieudanych operacji

### Logger (System logowania)
- Zapewnia spójne logowanie w całej aplikacji
- Zbiera informacje o działaniu systemu
- Pomaga w debugowaniu i monitorowaniu

### Monitoring (System monitoringu)
- Zbiera metryki wydajnościowe
- Generuje alerty i raporty
- Pomaga w monitorowaniu stanu systemu



## Frontend

### UI (Interfejs użytkownika)
- Główny komponent odpowiedzialny za interakcję z użytkownikiem
- Komunikuje się z backendem poprzez API Gateway używając HTTP/REST

### PizzaSelector (Komponent wyboru składników)
- Pozwala użytkownikowi na wybór składników pizzy
- Przekazuje wybrane kryteria do listy wyników

### ResultsList (Lista wyników)
- Wyświetla wyniki wyszukiwania
- Umożliwia przejście do szczegółów restauracji

### RestaurantDetails (Szczegóły restauracji)
- Pokazuje szczegółowe informacje o wybranej restauracji
- Wyświetla menu i dostępne opcje


## Backend

### APIGateway
- Główny punkt wejścia dla wszystkich żądań
- Integruje się z serwisami AuthService, ErrorHandler i Logger

### PizzaService (Serwis Pizzy)
- Zarządza logiką związaną z pizzami
- Komunikuje się z serwisem restauracji

### RestaurantService (Serwis Restauracji)
- Obsługuje informacje o restauracjach
- Przekazuje żądania do serwisu wyszukiwania

### SearchService (Serwis Wyszukiwania)
- Implementuje logikę wyszukiwania
- Korzysta z walidatora danych
- Zarządza cache'm i bazą danych

### DataValidator (Walidator danych)
- Sprawdza poprawność danych
- Współpracuje z adapterem Pyszne.pl

### IntegrationService (Serwis Integracji)
- Zarządza komunikacją z różnymi dostawcami
- Implementuje wzorzec fasady dla wszystkich integracji
- Obsługuje równoległe zapytania do wielu dostawców
- Agreguje i normalizuje wyniki
- Implementuje strategie fallback i circuit breaker

### Strategia obsługi błędów
- Jeśli jeden z dostawców jest niedostępny, system kontynuuje działanie z pozostałymi
- Implementacja circuit breaker zapobiega przeciążeniu niesprawnych API
- Cachowanie pozwala na dostarczenie podstawowych danych nawet przy awarii dostawcy

### UserService (Serwis Użytkownika)
- Zarządza logiką związaną z użytkownikami
- Komunikuje się z bazą danych

### FavoritesService (Serwis Ulubionych)
- Zarządza logiką związaną z ulubionymi pizzami
- Komunikuje się z bazą danych

### IntegrationService (Serwis Integracji)
- Zarządza komunikacją z różnymi dostawcami
- Implementuje wzorzec fasady dla wszystkich integracji
- Obsługuje równoległe zapytania do wielu dostawców
- Agreguje i normalizuje wyniki
- Implementuje strategie fallback i circuit breaker


## Data Layer

### Integration Layer
- Zarządza komunikacją z różnymi dostawcami
- Implementuje wzorzec fasady dla wszystkich integracji
- Obsługuje równoległe zapytania do wielu dostawców
- Agreguje i normalizuje wyniki
- Implementuje strategie fallback i circuit breaker


### Cache (Redis Cache)
- Przechowuje popularne wyniki wyszukiwania
- Przyspiesza odpowiedzi dla częstych zapytań

### Database (Baza danych)
- Przechowuje trwałe dane aplikacji
- Obsługiwana przez PostgreSQL


### Cache Strategy (Strategia cachowania)
#### L1 Cache (Cache aplikacyjny)
- Lokalny cache w pamięci aplikacji
- Najszybszy dostęp do danych
- Przechowuje często używane dane
- Automatyczne czyszczenie po określonym czasie

#### L2 Cache (Cache rozproszony)
- Implementowany przez Redis
- Współdzielony między instancjami
- Przechowuje większe zestawy danych
- Zaawansowane strategie invalidacji

## Monitoring
  
### System monitoringu
- Zbieranie metryk wydajnościowych
- Monitorowanie SLA
- Alerty przy przekroczeniu progów
- Dashboardy z metrykami w czasie rzeczywistym

#### Monitorowane metryki
- Czas odpowiedzi API
- Wykorzystanie zasobów
- Liczba równoczesnych użytkowników
- Cache hit ratio
- Błędy i wyjątki
- Dostępność usług

#### Alerty
- Przekroczenie czasu odpowiedzi (>2s)
- Wysoki poziom błędów
- Problemy z dostępnością
- Przeciążenie systemu

#### Health Checks
- Regularne sprawdzanie stanu usług
- Automatyczne wykrywanie problemów
- Integracja z systemem alertów



# Diagram przypadków użycia


   ```mermaid  
   flowchart LR
      subgraph Actors
         Anonymous[/Użytkownik Anonimowy/]
         User[/Użytkownik Zarejestrowany/]
         Admin[/Administrator/]
      end

      subgraph Pizza Finder System
         Register(Utworzenie konta)

         SearchPizza[Wyszukiwanie pizzy]
         FilterResults(Filtrowanie wyników)
         SortResults(Sortowanie wyników)
         ViewDetails(Przeglądanie szczegółów)

         EditProfile(Edycja profilu)
         ManageFavorites(Zarządzanie ulubionymi)
         ManagePreferences(Zarządzanie preferencjami)
         ViewHistory(Przeglądanie historii)

         Login(Logowanie)
         ManageSystem[Zarządzanie systemem]
         MonitorPerformance[Monitoring wydajności]
         ManageUsers[Zarządzanie użytkownikami]
      end

      %% Anonymous User connections
      Anonymous --> SearchPizza
      Anonymous --> FilterResults
      Anonymous --> SortResults
      Anonymous --> ViewDetails
      Anonymous --> Register

      %% User connections
      User --> Login
      User --> EditProfile
      User --> ManagePreferences
      User --> SearchPizza
      User --> FilterResults
      User --> SortResults
      User --> ViewDetails
      User --> ManageFavorites
      User --> ViewHistory

      %% Admin connections
      Admin --> ManageSystem
      Admin --> MonitorPerformance
      Admin --> ManageUsers
      Admin --> Login
   ```


# Diagramy stanu
## System logowania
```mermaid
   stateDiagram-v2
    [*] --> Anonimowy
    Anonimowy --> Rejestracja : Rejestracja konta
    Anonimowy --> Logowanie : Logowanie
    Logowanie --> Anonimowy : Powrót do stanu anonimowego
    Rejestracja --> Logowanie : Logowanie
    Logowanie --> Zalogowany : Zalogowanie udane
    Zalogowany --> Wylogowanie : Wylogowanie
    Wylogowanie --> Anonimowy : Powrót do stanu anonimowego
    Zalogowany --> [*] : Zakończenie
```

## System wyboru pizzy
```mermaid
stateDiagram-v2
    [*] --> PrzeglądaniePizzy
    PrzeglądaniePizzy --> Wyszukiwanie : Wyszukiwanie pizzy
    Wyszukiwanie --> Filtrowanie : Filtrowanie wyników
    Filtrowanie --> Sortowanie : Sortowanie wyników
    Sortowanie --> Szczegóły : Przeglądanie szczegółów pizzy
    Szczegóły --> Ulubione : Dodaj do ulubionych
    Ulubione --> PrzeglądaniePizzy : Powrót do wyników
    Szczegóły --> [*] : Powrót do głównego ekranu
```

## System ulubionych
```mermaid
stateDiagram-v2
    [*] --> BrakUlubionych
    BrakUlubionych --> UlubionePizze : Dodaj pizzę do ulubionych
    UlubionePizze --> Usuwanie : Usuwanie z ulubionych
    Usuwanie --> BrakUlubionych : Brak ulubionych
    UlubionePizze --> [*] : Zakończenie
```


# Diagramy sekwencji

## Logowanie użytkownika
```mermaid
sequenceDiagram
    participant User
    participant UI
    participant APIGateway
    participant AuthService
    participant Database

    User->>UI: Enter login details (email, password)
    UI->>APIGateway: Send login request (email, password)
    
    alt If no internet connection
        APIGateway-->>UI: Show error message (no internet connection)
        UI-->>User: Display error (no internet connection)

    end

    APIGateway->>AuthService: Validate login credentials
    
    alt If input is incomplete or invalid
        AuthService-->>APIGateway: Return error (missing fields)
        APIGateway-->>UI: Show error message (invalid input)
        UI->>User: Display error (missing email or password)
    end
    
    APIGateway->>AuthService: Fetch user by email
    alt If Database is unavailable
        AuthService-->>APIGateway: Return error (Database connection failed)
        APIGateway-->>UI: Show error message (database failure)
        UI->>User: Display error (please try again later)
    end
    
    Database-->>AuthService: Return user data
    
    alt If user does not exist
        AuthService-->>APIGateway: Return error (user not found)
        APIGateway-->>UI: Show error message (user not found)
        UI->>User: Display error (user does not exist)
    end
    
    AuthService->>AuthService: Verify password
    
    alt If credentials are invalid
        AuthService-->>APIGateway: Return error (invalid credentials)
        APIGateway-->>UI: Show login error message
        UI->>User: Display login error (incorrect email or password)
    end
    
    AuthService->>AuthService: Generate JWT Token
    
    alt If JWT token generation fails
        AuthService-->>APIGateway: Return error (JWT generation failed)
        APIGateway-->>UI: Show error message (please try again)
        UI->>User: Display error (unable to log in, please try again later)
    end
    
    AuthService-->>APIGateway: Return JWT Token (authentication success)
    APIGateway-->>UI: Send JWT Token (authentication success)
    UI->>User: Show logged in state

```

##  Wyszukanie pizzy

```mermaid
sequenceDiagram
    participant User
    participant UI
    participant APIGateway
    participant AuthService
    participant PizzaService


    User->>UI: Search for pizza
    UI->>APIGateway: Send search request (pizza name, filters)
    APIGateway->>PizzaService: Fetch pizza details
    PizzaService->>PizzaService: Search pizzas in DB
    PizzaService-->>APIGateway: Return list of pizzas
    APIGateway-->>UI: Send search results
    UI->>User: Display pizza results
```


# Diagram czynności

```mermaid
stateDiagram-v2
    [*] --> SearchingForPizza

    SearchingForPizza --> PizzasFound : Search successful
    SearchingForPizza --> NoPizzasFound : No results found

    PizzasFound --> Filtering : User selects ingredients filter
    Filtering --> FilteringResults : Filter applied
    Filtering --> PizzasFound : User cancels filter

    PizzasFound --> Sorting : User selects sorting criteria
    Sorting --> SortedResults : Sorting applied
    Sorting --> PizzasFound : User cancels sorting

    PizzasFound --> DisplayingDetails : User selects pizza
    Sorting --> DisplayingDetails : User selects pizza after sorting
    Filtering --> DisplayingDetails : User selects pizza after filtering
    DisplayingDetails --> [*] : Pizza details viewed

    NoPizzasFound --> [*] : No action



```


# Diagram klas dla osoby nietechnicznej


```mermaid
---
  config:
    class:
      hideEmptyMembersBox: true
---

classDiagram

    class Annonymous {

    }




    class User {
        + name
        + email
        - password
        + favorite pizzas
    }

    class API_Pyszne {
    }
    class Pizza {
        + name
        + ingredients
        + price
        + description
        + rating
        + delivery time
        + restaurant
    }

    class Restaurant {
        + name
        + location
        + menu
        + rating
        + delivery time
    }

    class Administrator {
        + name
        + email
        - password
    }



    Restaurant --> Pizza : menu
    Annonymous --> User : can create
    API_Pyszne --> Restaurant : sends data about
    User <|-- Administrator : can delete 
    User --> Pizza : search/manage favorites
    Annonymous --> Pizza : search
```

#
#
#
#

```mermaid
classDiagram
    class User {
        +String id
        +String name
        +String email
        -String password
        +List~Pizza~ favorites
        +searchPizzas(String query)
        +filterPizzas(Map~String, String~ filters)
        +sortPizzas(String criteria)
        +getPizzaDetails(String pizzaId)
        +addToFavorites(Pizza pizza)
        +removeFromFavorites(Pizza pizza)
    }

    class Pizza {
        +String id
        +String name
        +List~String~ ingredients
        +double price
        +String restaurantId
        +String description
        +double rating
        +int deliveryTime
    }

    class Restaurant {
        +String id
        +String name
        +String location
        +List~Pizza~ menu
        +double rating
        +int deliveryTime
        +getMenu()
        +getDetails()
    }

    class AuthService {
        +String generateToken(User user)
        +boolean validateToken(String token)
        +User authenticate(String email, String password)
    }

    class PizzaService {
        +List~Pizza~ searchPizzas(String query)
        +List~Pizza~ filterPizzas(Map~String, String~ filters)
        +List~Pizza~ sortPizzas(String criteria)
        +Pizza getPizzaDetails(String pizzaId)
    }

    class UserService {
        +User createUser(String name, String email, String password)
        +User getUser(String id)
        +User updateUser(String id, Map~String, String~ updates)
        +boolean deleteUser(String id)
    }

    class FavoritesService {
        +List~Pizza~ getFavorites(String userId)
        +boolean addToFavorites(String userId, Pizza pizza)
        +boolean removeFromFavorites(String userId, Pizza pizza)
    }

    class Administrator {
        +String id
        +String name
        +String email
        -String password
        +manageUsers()
        +manageRestaurants()
    }

    User <|-- Administrator

    User --> PizzaService : uses
    User --> FavoritesService : uses
    User --> AuthService : uses
    PizzaService --> Pizza : manages
    Restaurant --> Pizza : contains
    UserService --> User : manages
    FavoritesService --> Pizza : manages
    AuthService --> User : authenticates
    Administrator --> UserService : manages
    Administrator --> Restaurant : manages
```