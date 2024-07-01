Lokalizacja i struktura katalogów
---------------------------------
Lokalizacja:
~~~~~~~~~~~~
1) Katalog danych:
    - ``/var/lib/postgresql/<wersja>/main`` na na systemach Debian/Ubuntu
    - ``/var/lib/pgsql/<wersja>/data`` na systemach Red Hat/CentOS.
    - Zawiera wszystkie dane, pliki konfiguracyjne, logi i pliki kontrolne.

2) Katalog Konfiguracyjny:
    - Pliki konfiguracyjne zwykle znajdują się w katalogu danych, choć zdarza się, że mogą znajdować się w innym katalogu np ``/etc/postgresql/<wersja>/main``

3) Katalog logów:
    - Domyślnie ``/var/log/postgresql`` na Debianie/Ubuntu oraz ``/var/lib/pgsql/<wersja>/data/pg_log`` na Red Har/CentOS
    - zawiera logi PostgreSQL

Struktura katalogów:
~~~~~~~~~~~~~~~~~~~~
- ``base/``: Zawiera dane użytkownika dla każdej bazy danych.
- ``global/``: Przechowuje dane globalne, np. tabele systemowe.
- ``pg_xlog/`` lub pg_wal/ (od wersji 10): Zawiera dzienniki Write-Ahead Log (WAL).
- ``pg_clog/`` lub pg_xact/: Przechowuje dane dotyczące transakcji.
- ``pg_tblspc/``: Linki symboliczne do tabel przestrzeni.
- ``pg_multixact/``: Dane dotyczące wielokrotnych transakcji.
- ``pg_subtrans/``: Dane dotyczące podrzędnych transakcji.
- ``pg_stat/``: Dane statystyczne.
- ``pg_snapshots/``: Przechowuje dane dotyczące snapshotów.


