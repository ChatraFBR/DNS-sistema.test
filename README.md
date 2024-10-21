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