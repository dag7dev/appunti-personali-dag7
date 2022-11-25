.. _rbac:

RBAC
====

Acronimo di Role-Binding-Access-Control.

Metodo per gestire accessi alle risorse, diviso in: - role: permessi -
role binding: chi puo farli e chi no - cluster role - cluster role
binding

Differenza tra role e clusterRole
---------------------------------

Ruolo: solo namespace ClusterRole: tutto, globale

All'interno di Yaml ho

.. code:: yaml

   rules:
   - apiGroups: [""] # tutto
     resources: ["pods"] #che tipo di risorsa può accedere
     verbs: ["get", "list"] #quali azioni sono permesse
