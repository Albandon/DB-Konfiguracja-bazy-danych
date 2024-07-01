.. role:: sql(code)
	:language: sql

Tabele - rozmiar, planowanie i monitorowanie
--------------------------------------------

Rozmiar
~~~~~~~

Rozmiar tabel jest kluczowym aspektem zarządzania bazą danych, ponieważ bezpośrednio wpływa na wydajność systemu. Duże tabele mogą prowadzić do dłuższych czasów odpowiedzi na zapytania, zwiększonego zużycia pamięci oraz większych obciążeń wejścia/wyjścia.

**Monitorowanie rozmiaru tabel**
PostgreSQL dostarcza kilka narzędzi i zapytań SQL, które umożliwiają monitorowanie rozmiaru tabel:

- **Rozmiar pojedynczej tabeli:**

:sql:`SELECT pg_size_pretty(pg_total_relation_size('nazwa_tabeli')) AS size;`

To zapytanie zwraca przyjazny dla użytkownika rozmiar całej tabeli, wliczając w to dane, indeksy oraz wszelkie towarzyszące pliki.

- **Rozmiar wszystkich tabel w bazie danych:**

.. code-block:: sql

	SELECT relname AS "Table", pg_size_pretty(pg_total_relation_size(relid)) AS "Size"
	FROM pg_catalog.pg_statio_user_tables
	ORDER BY pg_total_relation_size(relid) DESC;

To zapytanie zwraca listę wszystkich tabel w bazie danych wraz z ich rozmiarami, posortowaną według rozmiaru.

- **Rozmiar indeksów tabeli:**

.. code-block:: sql

	SELECT indexrelname AS "Index", pg_size_pretty(pg_relation_size(indexrelid)) AS "Size"
	FROM pg_stat_user_indexes
	WHERE relname = 'nazwa_tabeli';

To zapytanie zwraca listę indeksów dla określonej tabeli wraz z ich rozmiarami.

Planowanie
~~~~~~~~~~

Planowanie tabel w PostgreSQL obejmuje kilka kluczowych aspektów, takich jak normalizacja, denormalizacja, indeksowanie oraz partycjonowanie.

1) **Normalizacja** - Normalizacja to proces organizowania danych w bazie danych w taki sposób, aby zminimalizować redundancję i zapewnić integralność danych. Składa się z kilku form normalnych (NF), z których każda eliminuje różne rodzaje redundancji:

	- **Pierwsza forma normalna (1NF)**: Każda komórka tabeli zawiera pojedynczą wartość, a każde pole jest atomowe.
	- **Druga forma normalna (2NF)**: Spełnia wymagania 1NF i zapewnia, że wszystkie kolumny niekluczowe są w pełni zależne od klucza głównego.
	- **Trzecia forma normalna (3NF)**: Spełnia wymagania 2NF i zapewnia, że nie ma zależności przejściowych między kolumnami niekluczowymi.

2) **Denormalizacja** - Denormalizacja to proces łączenia tabel, aby poprawić wydajność zapytań kosztem zwiększenia redundancji danych. Stosuje się ją wtedy, gdy zapytania często wymagają złączeń wielu tabel, co może wpływać na wydajność.

3) **Indeksowanie** - Indeksy są kluczowe dla optymalizacji wydajności zapytań. Pozwalają one na szybsze wyszukiwanie, sortowanie i filtrowanie danych. W PostgreSQL dostępnych jest kilka typów indeksów:

	- **B-tree**: Najczęściej używany typ indeksu, odpowiedni dla większości operacji wyszukiwania.
	- **Hash**: Szybszy dla operacji równościowych, ale mniej wszechstronny niż B-tree.
	- **GIN (Generalized Inverted Index)**: Używany do indeksowania danych wielowartościowych, takich jak tabele JSONB i dokumenty pełnotekstowe.
	- **GiST (Generalized Search Tree)**: Używany dla bardziej złożonych typów danych, takich jak geometria.

Przykład tworzenia indeksu B-tree:

:sql:`CREATE INDEX idx_nazwa_kolumny ON nazwa_tabeli(nazwa_kolumny);`

4) **Partycjonowanie** - Partycjonowanie polega na podzieleniu tabeli na mniejsze, bardziej zarządzalne części zwane partycjami. Może to znacznie poprawić wydajność zapytań, szczególnie w przypadku dużych tabel. PostgreSQL obsługuje kilka typów partycjonowania:

	- **Partycjonowanie zakresowe (Range Partitioning)**: Dzieli dane na zakresy wartości.
	- **Partycjonowanie listy (List Partitioning)**: Dzieli dane na podstawie wartości z określonej listy.
	- **Partycjonowanie haszowe (Hash Partitioning)**: Dzieli dane na podstawie wartości haszowej.

Przykład tworzenia partycji zakresowej:

.. code-block:: sql

	CREATE TABLE orders (
   	order_id serial PRIMARY KEY,
    	order_date date NOT NULL,
    	customer_id int NOT NULL,
    	amount numeric
	) PARTITION BY RANGE (order_date);
	CREATE TABLE orders_2022 PARTITION OF orders
    	FOR VALUES FROM ('2022-01-01') TO ('2023-01-01');
	CREATE TABLE orders_2023 PARTITION OF orders
    	FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');

Monitorowanie
~~~~~~~~~~~~~

Dodatkowo możlive jest monitorowanie wydajności tabel za pomocą:

- **pg_stat_user_table:**

.. code-block:: sql

	SELECT relname, seq_scan, seq_tup_read, idx_scan, idx_tup_fetch, n_tup_ins, n_tup_upd, n_tup_del
	FROM pg_stat_user_tables;

- **pg_stat_activity:**

.. code-block:: sql

	SELECT pid, usename, datname, state, query_start, query
	FROM pg_stat_activity;
