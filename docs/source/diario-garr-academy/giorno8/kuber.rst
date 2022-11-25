.. _kuber:

Kubernetes
==========

Introduzione
------------

È un orchestratore con un sacco di strumenti e funzioni utili.

Ha questa serie di servizi:
- scaling
- updates
- load balancer
- monitoring
- service discovery

Per spiegarlo con metafora: 
- container: mucca
- immagine: formina

Se lasciamo pascolare troppe mucche nel recinto, le mucche soffrono perché non hanno abbastanza cibo.

Il rancher ha il compito di distribuire ed equilibrare il numero di mucche nei recinti

Come è fatto
------------

Cluster
~~~~~~~

Un cluster è un insieme di hardware e software.

Nella parte hardware abbiamo: - nodo **master**: che contiene o
contengono il control plane, ed è lui che gestisce il tutto -
**worker**: i nodi che runnano le varie applicazioni

Cluster ip
^^^^^^^^^^

-  Ogni servizio ha un ip.
-  È interno a loro.
-  Serve per far dialogare piu pods tra di loro.

   -  A noi non interessa.

Control plane
~~~~~~~~~~~~~

è uno dei nodi del cluster che gestiscono i nostri container all'interno
del worker e mantiene lo stato del cluster

Componenti software del control plane: - **API Server** - **etcd**:
mantiene lo stato di ogni cosa per ogni cluster. È uno storage di dati
che mantiene i dati in versione dizionario k:v - **scheduler**: ha il
compito di selezionare al giusto nodO; un certo container - **controller
manager**: viene usato per monitorare il cluster - esempio: il numero di
repliche per un determinato servizio

Worker
~~~~~~

-  **kubelet**: fa comunicare il worker con il control plane e lo fa
   tramite l'API server recuperando tutte le informazioni che gli
   servono
-  **kube proxy**: tutte le regole di routing, tutte le regole di
   comunicazione con gli altri container
-  **container runtime**: docker o containerd, software che ci permette
   di tirare su un conteiner

Plugin
------

CRI: container runtime interface
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

CRI runna su tutti i nodi ed è un plugin che permette la comunicazione
tra kubelet e containerd.

CNI: container network interface
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Fa modifiche a livello rete dei nostri host: ip, indirizzi vari…

CSI: container storage interface
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

L'ultimo strato: permette di gestire lo storage dei vari container.

Pod
---

È un container che anche lui ha un indirizzo IP al quale posso attaccare
dei volumi e al quale posso attaccare altri container.

Condividono la stessa interfaccia di rete.

   I container girano solo all'interno dei pod

Container pause: un container speciale che permette la condivisione
dell'interfaccia di rete.

Recap
~~~~~

-  Un servizio ci permette di esporre un app che gira su un insieme di
   pod attraverso ClusterIP (ip privato).
-  Quando viene creato un servizio al quale viene associato l'indirizzo
   ip, kubeproxy va a modificare all'interno del nodo del cluster
   netfilter (ha il compito di gestire i pacchetti) aggiornando l'ip del
   servizio con l'ip di un pod.
-  …


Guida
-----

.. code:: bash

   k explain <risorsa>
   # ad esempio pods

Bashrc: autocompletamento
-------------------------
Se vogliamo attivare l'autocompletamento:

.. code:: bash

   alias k=kubectl
   complete -o default -F __start_kubectl k
   #alias kubectl="minikube kubectl --"
   source <(kubectl completion bash)

Comandi
-------

Primo run
~~~~~~~~~

.. code:: bash

   minikube start --nodes 3 -p garr-academy

Che nodi
~~~~~~~~

::

   kubectl get nodes

Status esteso
~~~~~~~~~~~~~

.. code:: bash

   kubectl get nodes -o wide

Namespace
---------

I namespace non sono altro che

get namespace
~~~~~~~~~~~~~

::

   k get ns

create namespace
~~~~~~~~~~~~~~~~

::

   k create ns <nome>

.. _pod-1:

Pod
---

I **pod** sono delle applicazioni.

history del pod / come sta il pod
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

   k -n garr-academy describe pod <nome_pod>

creare un pod
~~~~~~~~~~~~~

.. code:: bash

   k -n garr-academy create -f hands-on_1_pod/pod.yaml

rimuovere un pod, cancellare un pod
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: bash

   k -n garr-academy delete pod <nomepod>

dove runna un'applicazione / dove il pod runna
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

   k -n garr-academy get pods -o wide

entrare nel pod
~~~~~~~~~~~~~~~

::

   k -n garr-academy exec -it pod/nginx bash

Minikube
--------

Minikube normalmente non si usa.

::

   minikube status -p garr-academy

"in breve che cos'è in esecuzione"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: bash

   minikube profile list

Config k8s
----------

Indovina un po'? YAML!

Su ``.kube/conf`` troviamo uno YAML fatto cosi: - Insieme dei cluster:
tutti i parametri dei cluster - ca - server - name - context: sezione
che mette in relazione il cluster e l'utente che vuole connettersi al
cluster. Es. voglio connettermi al cluster solo in qualita di reader. -
name - cluster - user - user - name - auth params, tokens …

Esempio:

.. code:: yaml

   apiVersion: v1
   clusters:
   - cluster:
       certificate-authority: /home/academy/.minikube/ca.crt
       extensions:
       - extension:
           last-update: Wed, 02 Nov 2022 11:17:09 CET
           provider: minikube.sigs.k8s.io
           version: v1.27.1
         name: cluster_info
       server: https://192.168.49.2:8443
     name: garr-academy
   contexts:
   - context:
       cluster: garr-academy
       extensions:
       - extension:
           last-update: Wed, 02 Nov 2022 11:17:09 CET
           provider: minikube.sigs.k8s.io
           version: v1.27.1
         name: context_info
       namespace: default
       user: garr-academy
     name: garr-academy
   current-context: garr-academy
   kind: Config
   preferences: {}
   users:
   - name: garr-academy
     user:
       client-certificate: /home/academy/.minikube/profiles/garr-academy/client.crt
       client-key: /home/academy/.minikube/profiles/garr-academy/client.key

Persistenza
-----------

Puoi creare dei volumi che poi puoi associare ad un cluster. Ogni
storage class definisce un tipo specifico di storage.

Per usare uno storage, devo usare un PVC, ossia un claim. È il discorso
del "permesso di scrivere".

Sono due file!

Con uno creiamo la risorsa volume, con l'altro la risorsa pod.

Creiamo la risorsa volume:

.. code:: yaml

   apiVersion: v1
   kind: PersistentVolumeClaim
   metadata:
     name: pvc-nginx
     namespace: garr-academy
   spec:
     storageClassName: standard
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 3Gi

Devo aggiungerlo nello yaml di configurazione del pod.

.. code:: yaml

   spec:
     volumes:
       - name: nginx-volume
         persistentVolumeClaim:
           claimName: pvc-nginx

       containers:
       - name: blablabla
           # cartella di mount, che fa da bridge col pod
           volumeMounts:
               - mountPath: "/garr-academy"
                 name: nginx-volume

check pvc - fare il check di un claim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

   k -n garr-academy get pvc

In breve…
---------

1. Crei i nodi
2. Crei i pod. Non ti interessa dove vanno messi, ci pensa lo scheduler
   non è affar tuo
3. Persistenza: il disco è permanente, la leghi un volume e quel volume
   è quello finché non lo distruggi

Servizi - esporre le porte
--------------------------

ClusterIP vs. Node Port vs. Load Balancer

Link: https://stackoverflow.com/a/52241241

Tutti e tre servono per esporre le porte, dal metodo piu semplice al piu
difficile.

-  **ClusterIP** devi dargli il port-forwarding esplicito da linea di
   comando, specificando le due porte da esporre (interne ed esterne)
-  **Nodeport** espone la porta su **TUTTI** i cluster. Per accedere
   devi scrivere l'IP e la porta "strana"
-  **Load balancer** dopo che gli assegna un IP pubblico, posso usarlo
   per accedere al contenuto della macchina

Ingress
-------

Aka "smistatutto": tipo un backend che a seconda dell'indirizzo che gli
diamo ridireziona il traffico sul cluster che vuole lui.

I servizi che si creano ovviamente sono clusterip, quelli più semplici,
perché tanto Ingress smista da solo e fa tutto lui.

Replicaset e deployment
-----------------------

Il compito di un replica è quello di copiare e mantenere lo stato
running delle repliche.

Nel mondo reale si usa il **deployment**, con il quale gestiamo il
rollback e le copie locali.

Deployment
----------

Get deployment
~~~~~~~~~~~~~~

::

   k -n garr-academy get deployments.apps 

Creare deployment - aggiornare deployment - settare deployment - fare l'apply:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

   k apply -f <file yaml> 

Vedi anche: [[Service Account]]
