Vagrant.configure("2") do |config|
  config.vm.box = "peru/ubuntu-18.04-desktop-amd64" 
  config.vm.box_version = "20190723.01"
  #config.vm.box = "ubuntu/bionic64"
  config.vm.hostname = "ubuntu-lts-dev"
    
  config.vm.provider "virtualbox" do |v|
    v.name = "ubuntu-lts-dev"
    v.gui = true
    v.memory = 12288
    v.cpus = 4
  end
  
 puts "Enter the username you would like to user: "
 user = STDIN.gets.chomp
 
 config.vm.provision "file", source: "eclipse.desktop", destination: "/tmp/eclipse.desktop"
 config.vm.provision "file", source: "soapui.desktop", destination: "/tmp/soapui.desktop"
 config.vm.provision "file", source: "keyboard", destination: "/tmp/keyboard"
 config.vm.provision "file", source: "setupMenu.sh", destination: "/tmp/setupMenu.sh"

 config.vm.provision :shell, inline: <<-SHELL
    #apt -y update
    apt -y install snapd

    mkdir /opt/tmp
    cp /tmp/eclipse.desktop /opt/tmp/eclipse.desktop
    cp /tmp/soapui.desktop /opt/tmp/soapui.desktop
    cp /tmp/keyboard /etc/default/keyboard

    #FIXME: Execute on Login.
    cp /tmp/setupMenu.sh /opt/resetMenu.sh

    # Install Gnome Desktop
    ## apt -y install ubuntu-desktop    
    
    # Install VMWare Guest Addons
    apt-get -y install virtualbox-guest-additions-iso
    
    # Install Java OpenJDK 
    cd /opt
    wget https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.4%2B11/OpenJDK11U-jdk_x64_linux_hotspot_11.0.4_11.tar.gz
    tar -xvzf /opt/OpenJDK11U-jdk_x64_linux_hotspot_11.0.4_11.tar.gz

    
    # Install Eclipse
    wget http://ftp.snt.utwente.nl/pub/software/eclipse/technology/epp/downloads/release/2019-09/M1/eclipse-jee-2019-09-M1-linux-gtk-x86_64.tar.gz
    tar xfz eclipse-jee-2019-09-M1-linux-gtk-x86_64.tar.gz

    cp /opt/tmp/eclipse.desktop /usr/share/applications/eclipse.desktop

    # Install Maven
    sudo apt-get -y install maven

    # Shell    
    sudo apt-get -y install screenfetch
    rm /usr/share/applications/ubuntu-amazon-default.desktop

    #de_CH

    # Install Java Oracle JDK 8 - switchable.
    # Install IntelliJ Open Source

    # plantuml server

    # Install SOAP UI
    cd /opt/
    wget https://s3.amazonaws.com/downloads.eviware/soapuios/5.5.0/SoapUI-5.5.0-linux-bin.tar.gz
    tar xfz SoapUI-5.5.0-linux-bin.tar.gz
    cp /opt/tmp/soapui.desktop /usr/share/applications/soapui.desktop

    # Install Talend ESB 7.2.1
    # Install zsh and powerline
    # Install Docker
    # Install visual code
    apt -y install software-properties-common apt-transport-https wget
    wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
    add-apt-repository -y "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
    apt -y install code

    # Dotfiles 
    # Setup Desktop Links
    # Setup Example projects
    # Setup Background
    # Set Dark Theme 
    # Setup all AUto complete stuff (beans.xml and stuff)
    # Setup all syntax highlighting stuff all editors     
    # Setup Dotfiles (incl. VIM)
    # setup and install i3
    # Setup Background Image.    

  SHELL

  config.vm.provision :shell, inline: "adduser #{user}"
  config.vm.provision :shell, inline: "passwd -d -u #{user}"
  config.vm.provision :shell, inline: "chage -d0 #{user}"  
  config.vm.provision :shell, inline: "usermod -aG sudo #{user}"  
  
  config.vm.provision :shell, inline: "echo 'export PATH=/opt/jdk-11.0.4+11/bin:\$PATH' >> /home/#{user}/.bashrc" 
  config.vm.provision :shell, inline: "echo 'export PATH=/opt/eclipse:\$PATH' >> /home/#{user}/.bashrc"  
  config.vm.provision :shell, inline: "echo 'screenfetch' >> /home/#{user}/.bashrc" 

  config.vm.provision :shell, inline: "reboot"  
end
