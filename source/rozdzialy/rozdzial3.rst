Podstawowe parametry konfiguracyjne
-----------------------------------

Plik postgresql.conf
~~~~~~~~~~~~~~~~~~~~

Plik *postgresql.conf* zawiera ustawienia dotyczące wydajności, logowania, sieci i wielu innych aspektów.

**Kluczowe ustawienia:**

1) **Słuchanie połączeń:**

::

	listen_addresses = 'localhost'  # Adresy IP, na których PostgreSQL będzie nasłuchiwać połączeń
	port = 5432                     # Port, na którym PostgreSQL będzie nasłuchiwać połączeń 


2) **Pamięć i wydajność:**

::

	shared_buffers = 128MB           # Ilość pamięci RAM przeznaczona na buforowanie danych
	work_mem = 4MB                   # Ilość pamięci RAM na operacje sortowania i agregacji na użytkownika
	maintenance_work_mem = 64MB      # Ilość pamięci RAM na operacje utrzymaniowe (np. VACUUM, CREATE INDEX)

3) **Autovacuum:**

::

	autovacuum = on                  # Automatyczne czyszczenie i analiza tabel
	autovacuum_naptime = 1min        # Częstotliwość uruchamiania procesu autovacuum


---------------------------------------------

Plik pg_hba.conf
~~~~~~~~~~~~~~~~

Plik **pg_hba.conf** odpowiada za kontrolę dostępu do bazy danych PostgreSQL.

**Przykład konfiguracji:**
::
	
	# TYPE  DATABASE        USER            ADDRESS                 METHOD

	# Zezwól lokalnym użytkownikom na połączenie
	local   all             all                                     md5

	# Zezwól zdalnym użytkownikom z sieci 192.168.1.0/24 na połączenie
	host    all             all             192.168.1.0/24          md5

Plik pg_ident.conf
~~~~~~~~~~~~~~~~~~

Plik **pg_ident.conf** pozwala mapować systemowych użytkowników do użytkowników PostgreSQL.

**Przykład konfiguracji:**

::

	# MAPNAME       SYSTEM-USERNAME         PG-USERNAME

	mymap           johndoe                 john
	mymap           janedoe                 jane

W pliku **pg_hba.conf** można użyć tej mapy:
::

	host    all             all             127.0.0.1/32            ident map=mymap

