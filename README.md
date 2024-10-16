# DNS-sistema.test

# Tarea realizada por:

**Fernando Bueno Rivera**

## Creadion de la tarea el dia 14/10/2024

# Creamos el archivo .gitignore en el cual escribimos el comando .vagrant

# En el archivo vagrantfile la configuramos de la siguiente manera

    config.ssh.insert_key = false

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