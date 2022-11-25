.. _jinja:

Jinja
=====

Jinja è un linguaggio di **templating**.

Idea:
   - KISS
   
   - molto simile a python 
   
   - misto tra html e "tipo php"

Costrutti
---------

``{{ }}`` espressione ``{% %}`` statement ``{# #}`` commento

Filtri
------

{{ var \| filter }} con i **filtri** possiamo **manipolare le
variabili** che gli passiamo.

Elementi da non renderizzare:

::

   {%raw}
   {%endraw}


Esempio:
::

   <html>  
      <body>  
      <h1>{{ _template_var1_ }}</h1>  
      <p>{{ _template_var2_ }}</p>  
     </body>  
   </html>

``_template_var1_`` e ``_template_var2_`` verranno sostituiti con i
valori che gli passo.
