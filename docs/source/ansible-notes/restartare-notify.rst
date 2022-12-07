Restartare qualcosa con notify e handlers
=========================================
In un ruolo Ansible si puo eseguire un pezzo se e solo se restarto.

Dentro il main nella cartella handlers:
.. code:: yaml
    - name: Restart haproxy
        service:
        name: haproxy
        state: restarted

E poi posso usarlo nei task:
.. code:: yaml
    - name: Copia cartelle con configurazioni
        ansible.builtin.template:
            src: "haproxy.cfg.j2"
            dest: "/etc/haproxy/haproxy.cfg"
            mode: "0755"
        vars:
            ingress_ip: 192.168.49.2
        notify:
            - Restart haproxy

In questo modo quando modifico quel file do conf, basta mettere il notify e mi esegue in automatico dalla cartella handlers il main.