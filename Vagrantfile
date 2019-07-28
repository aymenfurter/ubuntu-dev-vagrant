Vagrant.configure("2") do |config|    
  config.vm.box = "ubuntu/bionic64"
  config.vm.hostname = "ubuntu-lts-dev"
    
  config.vm.provider "virtualbox" do |v|
    v.name = "ubuntu-lts-dev"
    v.gui = true
    v.memory = 12288
    v.cpus = 4
  end
  
 puts "Enter the username you would like to use: "
 user = STDIN.gets.chomp
 
 config.vm.provision "file", source: "eclipse.desktop", destination: "/tmp/eclipse.desktop"
 config.vm.provision "file", source: "soapui.desktop", destination: "/tmp/soapui.desktop"
 config.vm.provision "file", source: "plantuml.desktop", destination: "/tmp/plantuml.desktop"
 config.vm.provision "file", source: "complete-setup.sh", destination: "/tmp/complete-setup.sh"
 config.vm.provision "file", source: "keyboard", destination: "/tmp/keyboard"
 config.vm.provision "file", source: "setupMenu.sh", destination: "/tmp/setupMenu.sh"
 config.vm.provision "file", source: "startSoapUI.sh", destination: "/tmp/startSoapUI.sh"

 config.vm.provision :shell, inline: <<-SHELL
    apt -y update
    apt -y install snapd
    apt -y install vim
    snap install postman
    snap install gimp

    mkdir /opt/tmp
    cp /tmp/eclipse.desktop /opt/tmp/eclipse.desktop
    cp /tmp/soapui.desktop /opt/tmp/soapui.desktop
    cp /tmp/plantuml.desktop /opt/tmp/plantuml.desktop
    cp /tmp/keyboard /etc/default/keyboard
    cp /tmp/startSoapUI.sh /opt/startSoapUI.sh    

    #FIXME: Execute on Login.
    cp /tmp/setupMenu.sh /opt/resetMenu.sh

    # Install Gnome Desktop
    ## apt -y install ubuntu-desktop    
    
    # Install VMWare Guest Addons
    apt-get -y install virtualbox-guest-additions-iso
    
    # Install Java OpenJDK 11
    cd /opt
    wget https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.4%2B11/OpenJDK11U-jdk_x64_linux_hotspot_11.0.4_11.tar.gz
    tar -xvzf /opt/OpenJDK11U-jdk_x64_linux_hotspot_11.0.4_11.tar.gz

    wget https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u222-b10/OpenJDK8U-jdk_x64_linux_hotspot_8u222b10.tar.gz
    tar -xvzf /opt/OpenJDK8U-jdk_x64_linux_hotspot_8u222b10.tar.gz

    # Install Eclipse
    wget http://ftp.snt.utwente.nl/pub/software/eclipse/technology/epp/downloads/release/2019-09/M1/eclipse-jee-2019-09-M1-linux-gtk-x86_64.tar.gz
    tar xfz eclipse-jee-2019-09-M1-linux-gtk-x86_64.tar.gz
    cp /opt/tmp/eclipse.desktop /usr/share/applications/eclipse.desktop

    # Install Maven
    sudo apt-get -y install maven

    # Shell    
    sudo apt-get -y install screenfetch
    rm /usr/share/applications/ubuntu-amazon-default.desktop

    # Install SOAP UI
    cd /opt/
    wget https://s3.amazonaws.com/downloads.eviware/soapuios/5.5.0/SoapUI-5.5.0-linux-bin.tar.gz
    tar xfz SoapUI-5.5.0-linux-bin.tar.gz
    cp /opt/tmp/soapui.desktop /usr/share/applications/soapui.desktop
    
    # Install Docker
    apt -y install docker.io
    docker run -d -p 8080:8080 plantuml/plantuml-server:tomcat
    cp /opt/tmp/plantuml.desktop /usr/share/applications/plantuml.desktop

    # VSCode
    apt -y install software-properties-common apt-transport-https wget
    wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
    add-apt-repository -y "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
    apt -y install code

    # - Install i3
    apt -y install i3

    # Todos:
    # - Setup Example projects  
    # - Setup Editors w/ Auto complete & Syntax Highlighting     
    # - Some Dotfiles Setup (incl. VIM & ls-alias)
    # - Tweak VM FS Size
       
  SHELL

  config.vm.provision :shell, inline: "adduser #{user}"
  config.vm.provision :shell, inline: "passwd -d -u #{user}"
  config.vm.provision :shell, inline: "chage -d0 #{user}"  
  config.vm.provision :shell, inline: "usermod -aG sudo #{user}"    

  # Default JDK is 8
  config.vm.provision :shell, inline: "echo 'export PATH=/opt/jdk-11.0.4+11/bin:\$PATH' >> /home/#{user}/.bashrc" 

  config.vm.provision :shell, inline: "echo 'export PATH=/opt/eclipse:\$PATH' >> /home/#{user}/.bashrc"  
  config.vm.provision :shell, inline: "echo 'screenfetch' >> /home/#{user}/.bashrc" 
  config.vm.provision :shell, inline: "cp /tmp/complete-setup.sh /home/#{user}/Desktop/complete-setup.sh" 
  config.vm.provision :shell, inline: "chmod +x /home/#{user}/Desktop/complete-setup.sh" 
  config.vm.provision :shell, inline: "chown -R #{user}:#{user} /opt/*" 
  
  # remove vagrant usre
  config.vm.provision :shell, inline: "reboot"  
end
