.. role:: sql(code)
	:language: sql

Tabele - rozmiar, planowanie i monitorowanie
--------------------------------------------

Rozmiar
~~~~~~~

Rozmiar tabel można monitorować za pomocą poniższych zapytań SQL:

- **Dla jednej tabeli:**

:sql:`SELECT pg_size_pretty(pg_total_relation_size('nazwa_tabeli'));`

- **Dla wszystkich tabel:**

.. code-block:: sql

	SELECT relname AS "Table", pg_size_pretty(pg_total_relation_size(relid)) AS "Size"
	FROM pg_catalog.pg_statio_user_tables
	ORDER BY pg_total_relation_size(relid) DESC;

Planowanie
~~~~~~~~~~

Częścią planowania tabel jest:

1) **Normalizacja** - Rozbijanie tabel na mniejsze by zmninimalizować powtarzanie danych i zależności.

2) **Denormalizacja** - Łączenie tabel w większe i wprowadzanie redundancji w celu przyśpieszenia czasu zapytań.

3) **Indeksowanie** - tworzenie indeksów w celu przyspieszenia przeszukiwań tabeli.

:sql:`CREATE INDEX idx_nazwa_kolumny ON nazwa_tabeli(nazwa_kolumny);`

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

