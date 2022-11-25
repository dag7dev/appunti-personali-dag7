.. _yaml:

YAML
====
YAML, un linguaggio per dare direttive.

> meglio fare YAML lungo ed essere chiaro che farne uno piccolo dove non si capisce niente

Sintassi
--------
::

    ---  inizio
    #    commento
    -    lista puntata
    []   array (lista di elementi)
    var: variabile

Dizionario:
::

    - chiave
	chiave:valore


Inserire del testo:
::

    variable: |
	nel mezzo nel cammin di nostra vita
	mi ritrovai in una selva oscura
	che la diritta via era smarrita


mentre testo lungo tutto su una linea:
::

    variable: >
        nel mezzo nel cammin di nostra vita
        mi ritrovai in una selva oscura
        che la diritta via era smarrita

Output: ``nel mezzo nel cammin di nostra vita mi ritrovai in una selva oscura che la diritta via era smarrita``