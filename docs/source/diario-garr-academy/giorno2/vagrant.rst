.. _vagrant:

Vagrant
=======

Vagrant è un **provisioning tool** ed è uno zucchero sintattico per la
gestione di VM.

**Parametrizza** una **VM**.

Provider vs Provisioning
------------------------

Virtualbox, KVM, VMWare, Hyper-V Ansible, Puppet, Docker, Chef

Approfondire:
https://www.redhat.com/en/topics/automation/what-is-provisioning


Comandi principali
------------------

``vagrant init``: inizializza un vagrantfile ``vagrant up/halt``:
**accende** o **spegne** la VM ``vagrant provision``: **esegue il
provisioner** configurato sulla VM ``vagrant ssh``: accesso da terminale
``vagrant suspend/resume/destroy``: sospende, riprende, distrugge
``vagrant rsync`` **sincronizza** i file della cartella condivisa da
/vagrant locale a /vagrant sul guest della VM.

Vagrant file
------------

Questo file speciale istruisce la vbox.

Solitamente:

::

   bs.vm.box = "debian/bullseye64"

ma dipende dalla distro che vuoi.

Ci sono diverse direttive che servono a fare diverse cose.

Syncare i file
--------------

``vagrant rsync`` : questo fa la pull dei file.

Il nome della cartella è specificato nel Vagrantfile.

Eseguire il provisioner
-----------------------

Per eseguire il provisioner: ``vagrant provision`` -> esegue provisioner
(shell) e, nel nostro esempio, installa nginx.

::

     config.vm.provision "shell", inline: "sudo apt-get install -y nginx"

Potremmo utilizare come provisioner ad esempio **Ansible**.
