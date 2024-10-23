# DNS-sistema.test

# Tarea realizada por:

**Fernando Bueno Rivera**

## Creadion de la tarea el dia 14/10/2024

# Creamos el archivo .gitignore en el cual escribimos el comando .vagrant

# En el archivo vagrantfile la configuramos de la siguiente manera

    config.vm.box = "debian/bookworm64"


    config.vm.define "venus" do |venus|
        venus.vm.hostname = "venus.deaw.test"
        venus.vm.network "private_network", ip: "192.168.57.102"
    end

    config.vm.define "tierra" do |tierra|
        tierra.vm.hostname = "tierra.deaw.test"
        tierra.vm.network "private_network", ip: "192.168.57.103"
    end

## Cambiaremos la ip en la linea de codigo de private_network a la ip desada

## y en una terminal pondremos vagrant up

# a continuacion entramos con las maquinas virtuales con el comando 
    vagrant ssh venus
    vagrant ssh tierra
## y en las maquinas virtuales en abas ponemos el comando de actualizar
    sudo apt-get update
## despues instalaremos el bind en abas
    sudo apt-get install bind9 bind9utils bind9-doc
## accedemos a la carpeta default y al archivo named
    cd /etc/default
    sudo nano named
## y cambiamos el option al siguiente
    OPTIONS = "-u bind -4"

## lo guardamos y nos salimos 

## accedemos a la carpeta bind y al named.conf.options
    cd bind
    sudo nano named.conf.options
## y cambiamos el dnssec-validation a yes

## despues editamos el archivo named.conf.options en ambas máquinas
    sudo nano /etc/bind/named.conf.options

### Añadimos la configuración ACL (lista de control de acceso)

    acl "trusted" {
    127.0.0.0/8;
    192.168.57.0/24;

    };

    options {
    
    allow-recursion { trusted; };
    
    };

## Configuramos  el servidor maestro ierra.sistema.test
### editando el archivo 
    sudo nano /etc/bind/named.conf.local


## configuramos las zonas 
    zone "sistema.test" {
        type master;
        file "/etc/bind/db.sistema.test";
        allow-transfer { 192.168.57.102; }; // IP del servidor esclavo
    };

    zone "57.168.192.in-addr.arpa" {
        type master;
        file "/etc/bind/db.192";
        allow-transfer { 192.168.57.102; }; // IP del servidor esclavo
    };

## creamos los archivos de zona 
    sudo cp /etc/bind/db.local /etc/bind/db.sistema.test
    sudo cp /etc/bind/db.127 /etc/bind/db.192


## entramos ahora en la máquina virtual "venus" y edita los archivos de zona

    sudo nano /etc/bind/named.conf.local

## añadimos las zonas esclavas 

    zone "sistema.test" {
       type slave;
        file "/var/cache/bind/db.sistema.test";
        masters { 192.168.57.103; }; // IP del servidor maestro
    };

    zone "57.168.192.in-addr.arpa" {
       type slave;
       file "/var/cache/bind/db.192";
       masters { 192.168.57.103; }; // IP del servidor maestro
    };
