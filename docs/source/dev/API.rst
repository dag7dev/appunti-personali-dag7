.. _api:

API
===

Cosa sono
---------
API sta per Application Programming Interface.

È l'**insieme delle funzioni e dei metodi** che possono essere richiamati da altri programmi per **manipolare e visualizzare dati** del programma che fornisce le API.

Best Practises
--------------
1. Usare i **JSON**: sono più veloci, più supportati e vengono usati quasi ovunque.

2. **Nomi anziché verbi negli endpoint**: questo perché già i metodi classici dell'HTTP hanno i verbi.

   * SI: localhost:8000/posts
   * NO: localhost:8000/getPosts

3. I **metodi** che tipicamente si utilizzano sono:

 * GET: ritorna dati
 * POST: inserisce nuovi dati
 * PUT: aggiorna dati già esistenti
 * DELETE: elimina dati

4. Quando trattiamo di collezioni usiamo **nomi al plurale**: i metodi classici dell'HTTP hanno già i verbi.

    SI: localhost:8000/posts/123

    NO: localhost:8000/post/123

5. Usa i **soliti classici codici di errore web** (100, 200...)

6. **Innesta gli endpoint** se occorre quando vuoi **mostrare relazioni**.
    ES: localhost/posts/postId/comments

**Il nesting deve essere <= 3**.

7. Permetti di **filtrare, ordinare e paginare** le risposte.


Vedi anche :ref:`rest`