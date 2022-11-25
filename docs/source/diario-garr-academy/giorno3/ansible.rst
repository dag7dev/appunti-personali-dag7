.. _ansible:

Ansible
=======

Introduzione
------------

Ansible aiuta ad **automatizzare** la **configurazione di server**
tramite **SSH**.

Schema
~~~~~~

-  Control zone

   -  **Inventory**: lista dei nodi
   -  **Ansible** (il software)

-  Nodi: server

Chore concepts
~~~~~~~~~~~~~~

Il linguaggio che Ansible usa: **YAML**

Inventory
---------

-  Inventory contiene tutti i soggetti che Ansible ha.

   -  Tendenzialmente sono file ``.ini``, posso anche usare dei file
      ``.yml``

Ci sono diversi inventory: ``/etc/ansible/hosts`` -> predefinito
``<proj-path>/inventory_test.ini`` -> test
``<proj-path>/inventory_prod.ini`` -> produzione
``<proj-path>/inventory_stage.ini`` -> area di stage

Struttura se ho modificato il file hosts della macchina locale
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Struttura (``inventory_test``) se ho modificato il file hosts:

.. code:: ini

   [stage]                  # [gruppo] lista di hosts
   stage1.garr.academy      
   stage2.garr.academy

Invece nell'hosts:

.. code:: hosts

   192.168.56.2        stage1.garr.academy        stage1
   192.168.56.3        stage2.garr.academy        stage2

Usare nomi simbolici senza dover modificare il file ``/etc/hosts``
------------------------------------------------------------------

**Piu comodo**, non bisogna farlo ogni volta che si aggiorna un host…
insomma, **basta usare questo**.

.. code:: ini

   [stage]                  # [gruppo] lista di hosts
   stage1.garr.academy      ansible_host=192.168.56.2
   stage2.garr.academy      ansible_host=192.168.56.3

Facts
-----

Informazioni reperite dai server host remoti e utilizzati come variabili
negli ansible playbook.

Tramite il comando:

.. code:: bash

   ansible stage1.garr.academy -m ansible.builtin.setup -i my-academy-project/my-academy-inv.ini --private-key .vagrant/machines/stage1/virtualbox/private_key -u vagrant

ci connettiamo all'host e otteniamo una serie di facts in json, di
informazioni utili!

Minutezza: ``export ANSIBLE_HOST_KEY_CHECKING=False`` -> usi questo per
**non fare in modo che Ansible controlli la key ogni volta**

Facts piu utilizzati
~~~~~~~~~~~~~~~~~~~~

Facts piu utilizzati: - ``"ansible_os_family":"Debian"`` - per usarlo in
un playbook o ovunque dovro usare ``{{ ansible_os_family }}``: proprio
come fosse una variabile. Infatti troveremo ‘Debian'. -
``ansible_default_ipv4['address']``

Variabili magiche
-----------------

Variabili speciali, che richiamandole danno accesso a delle
informazioni.

Sono comode.

Per approfondire:
https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html

Le piu importanti: - ``{{ hostvars }}``: tutti i server/host presenti e
le variabili assegnate a loro - ``{{ inventory_name }}``: su chi sto
eseguendo la ricetta? host - ``{{ groups }}``: tutti i gruppi presenti
nell'inventory e server/host sotto di loro - ``{{ group_names }}``:
tutti i gruppi a cui appartiene host per cui si sta eseguendo il
playbook

Playbook
--------

File yaml che viene eseguito per far girare Ansible.

Funziona a stati.

Ad esempio per installare un pacchetto possiamo usare il builtin di
Ansible, apt:

.. code:: yaml

   tasks:
       - name: Ensure apache is at the latest version
       ansibile.builtin.apt:
           name: apache2
           state: latest   # parola magica che indica installa l'ultima versione
                           # remove: absent

Il comando per eseguire un playbook:

.. code:: bash

   ansible-playbook my-academy-playbook.yml -i my-academy-inv.ini --private-key=$HOME/vagrant4academy/.vagrant/machines/stage1/virtualbox/private_key

verbose mode
~~~~~~~~~~~~

Quando si lancia il comando ansible-playbook, possiamo fargli stampare
cose di debug attraverso l'opzione ``-v`` Piu ``v`` inserisco, maggiore
sara' la verbosity

SSH
---

Per usare un playbook devo essere autenticato con SSH, tramite private
key.

Per farlo, glielo posso fare come parametro a ansible-playbook oppure
possiamo mettere nell'inventory quel percorso.

Allora avro l'Inventory:

.. code:: ini

   [stage]
   stage1.garr.academy     ansible_host=192.168.56.2       ansible_ssh_private_key_file=/home/academy/vagrant4academy/.vagrant/machines/stage1/virtualbox/private_key

   stage2.garr.academy     ansible_host=192.168.56.3       ansible_ssh_private_key_file=/home/academy/vagrant4academy/.vagrant/machines/stage2/virtualbox/private_key

Moduli
------

I moduli sono librerie Python che fanno in modo di eseguire determinate
operazioni su un qualche nodo.

Quelli di ansible.builtin fanno parte della collezione predefinita.

Builtin importanti
~~~~~~~~~~~~~~~~~~

I piu importanti: - ``systemd`` - ``copy`` - ``pip`` - ``command`` -
``import_task``: importa i task da un altro file

Importare i task
~~~~~~~~~~~~~~~~

Il file principale:

.. code:: yml

   tasks:
   - name: "Importa i task"
     ansible.builtin.import_tasks: all-tasks.yml

L'altro file con i moduli:

.. code:: yml

   - ansible.builtin.debug:
       msg: "{{ ansible_default_ipv4['address'] }} ha {{ ansible_distribution }}"
   #  - ansible.builtin.apt:
   #      name: python3-pip
   #      state: latest
   #      update_cache: yes
   #  - ansible.builtin.pip:
   #      name: cowsay
   - ansible.builtin.command:
       cmd: "{{ discovered_interpreter_python }} -m cowsay Hello Garr Academy!"

Loops
-----

Servono per eseguire task piu volte.

**Deve** essere usato **item**, se no, non funziona nulla e il corpo del
loop non viene eseguito.

Concetto di foreach (array semplice):

.. code:: yml

   - name: Create users
       ansible.builtin.user:
           name: `{{ item }}`
           state: present
       loop:
           - vittoria
           - andrea
           - stefano
           - ...

Concetto di foreach (array):

.. code:: yml

   - name: Create users
       ansible.builtin.user:
           name: `{{ item['name'] }}`
           uid: `{{ item['uid'] }}`
           groups: `{{ item['groups'] }}`
           state: present
       loop:
           - { name: 'vittoria', uid: 1001, groups:'devOps'}
           - ...

Condizionali
------------

When
~~~~

Es. ``when: ansible_facts['os_family'] == "Debian"``

Register
~~~~~~~~

Salva l'output di un modulo e permette di riutilizzarlo in altri moduli
es. ``register: python_version`` <- dentro python_version ci metto tutto
quello che gli ho detto di fare con ``ansible.builtin...``

Ansible Vault
-------------

Un metodo per criptare i file, bisogna digitare una passphrase e puo
essere fatto da file o da cmdline.

Abbiamo usato:

.. code:: bash

   ansible-vault encrypt --vault-password-file .vault_pass.txt my-academy-inv.ini

Se voglio invece eseguire il vault con il file ini criptato, basta
aggiungere ``--ask-vault-pass``

Gestione degli errori
---------------------

Chiavi speciali che modificano l'esecuzione del playbook. - ``default``:
se errore, blocca la macchina che da errore, il resto vai -
``any_errors_fatal: true``: se si verifica un errore, ferma tutto il
parco macchine. Non si mette all'interno dei task. -
``ignore_errors: true`` se il playbook ha un errorcode diverso da 0,
anche se c'e' un errore lo ignora. Si mette all'interno dei task. -
``failed_when``: a una specifica condizione il playbook si ferma

Ansible Galaxy
--------------

**Ansible Galaxy**: server di distribuzione di contenuti Ansible
(https://galaxy.ansible.com).

Troviamo sia roles che collections.

Roles e collections
-------------------

-  **Collections**: come vengono distribuiti i contenuti Ansible
   (playbook, rules, moduli, plugin). Sono dei depositi, dei container.
   Hanno una serie di roles.
-  **Roles**: una specie di grande script (ma ha delle cartelle) che fa
   mille cose, come settare un web server

Prima di riscrivere un role, sempre controllare che gia' ne esiste uno.

Per usarli nel playbook:

.. code:: yaml

   - name: Creazione di file sulle VM
   hosts: stage
   become: yes
   remote_user: vagrant

   roles:
   - nginxinc.nginx

Analisi dei roles
~~~~~~~~~~~~~~~~~

Vanno inseriti sempre nella cartella roles. Dentro ci troviamo otto
cartelle. - defaults - files - handlers - meta - tasks - templates -
tests - vars

Custom roles
~~~~~~~~~~~~

Possiamo creare anche noi i nostri ruoli per fare cose.
