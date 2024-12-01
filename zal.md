sprawozdanie, podpisac

```mermaid
stateDiagram-v2
    [*] --> Dostępna
    Dostępna --> Zarezerwowana : Rezerwacja
    Zarezerwowana --> Dostępna : Anulowanie rezerwacji
    Zarezerwowana --> Wypożyczona : Wypożyczenie
    Wypożyczona --> [*] : Zwrócenie książki
    Wypożyczona --> Dostępna : Anulowanie wypożyczenia

```