Vagrant.configure("2") do |config|      
  config.vm.box = "ubuntu/bionic64"  
  config.vm.hostname = "ubuntu-dev"
    
  config.vm.provider "virtualbox" do |v|
    v.name = "ubuntu-dev"
    v.gui = true
    v.memory = 12288
    v.cpus = 12
  end
  
 puts "Enter the username you would like to use: "
 user = STDIN.gets.chomp
 
 config.vm.provision "file", source: "eclipse.desktop", destination: "/tmp/eclipse.desktop"
 config.vm.provision "file", source: "soapui.desktop", destination: "/tmp/soapui.desktop"
 config.vm.provision "file", source: "plantuml.desktop", destination: "/tmp/plantuml.desktop"
  config.vm.provision "file", source: "keyboard", destination: "/tmp/keyboard"
 config.vm.provision "file", source: "i3-config", destination: "/tmp/i3-config"
 config.vm.provision "file", source: "gtk.css", destination: "/tmp/gtk.css" 
 config.vm.provision "file", source: "gtk-settings.ini", destination: "/tmp/gtk-settings.ini" 
 config.vm.provision "file", source: "setupMenu.sh", destination: "/tmp/setupMenu.sh"
 config.vm.provision "file", source: "startSoapUI.sh", destination: "/tmp/startSoapUI.sh"

 config.vm.provision :shell, inline: <<-SHELL
    #: '
    apt -y update
    apt -y upgrade
    apt -y install snapd
    apt -y install vim
    snap install postman
    snap install gimp
   
    mkdir /opt/tmp
    #'
    cp /tmp/eclipse.desktop /opt/tmp/eclipse.desktop
    cp /tmp/soapui.desktop /opt/tmp/soapui.desktop
    cp /tmp/plantuml.desktop /opt/tmp/plantuml.desktop
    cp /tmp/keyboard /etc/default/keyboard    
    cp /tmp/startSoapUI.sh /opt/startSoapUI.sh    
    cp /tmp/i3-config /opt/i3-config    
    cp /tmp/gtk.css /opt/gtk.css    
    cp /tmp/gtk-settings.ini /opt/gtk-settings.ini
    
    #: '
    #FIXME: Execute on Login.
    cp /tmp/setupMenu.sh /opt/resetMenu.sh

    # Install Gnome Desktop
    apt -y install ubuntu-desktop    
    
    # Install VMWare Guest Addons
    apt-get -y install virtualbox-guest-additions-iso virtualbox-guest-dkms virtualbox-guest-x11
    
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
    sudo apt-get install nitrogen

    cd /tmp
    wget https://download.virtualbox.org/virtualbox/6.0.14/VBoxGuestAdditions_6.0.14.iso
    sudo apt-get install p7zip-full
    7z x VBoxGuestAdditions_6.0.14.iso
    chmod +x VBoxLInuxAdditions.run
    ./VBoxLinuxAdditions.run

    cd ~
    git clone https://github.com/yshui/picom
    cd picom
    sudo apt-get install -y meson ninja-build libjson-c-dev libev-dev libxext-dev libxcb1-dev libxcb-damage0-dev libxcb-xfixes0-dev libxcb-shape0-dev libxcb-render-util0-dev libxcb-render0-dev libxcb-randr0-dev libxcb-composite0-dev libxcb-image0-dev libxcb-present-dev libxcb-xinerama0-dev libpixman-1-dev libdbus-1-dev libconfig-dev libxdg-basedir-dev libgl1-mesa-dev  libpcre2-dev  libevdev-dev uthash-dev libev-dev
    sudo apt-get install -y gcc-8 pkg-config
    sudo apt-get install -y libx11-xcb-dev xcb libxcb-image0 libxcb-image0-dev libpixman-1-dev libxcb-sync-dev  libxcb-composite0-dev libxcb-xinerama0 libxcb-xinerama0-dev libxcb-present-dev uthash-dev libconfig-dev libxdg-basedir-dev libpcre++-dev
    sudo apt-get install -y freeglut3 freeglut3-dev libdbus-1-dev
    sudo ninja -C build install
    sudo apt-get install -y g++-8
    #'
    #sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 800 --slave /usr/bin/g++ g++ /usr/bin/g++-8    
    #meson --buildtype=release . build

    #i3-gaps
    sudo apt install -y libxcb1-dev libxcb-keysyms1-dev libpango1.0-dev libxcb-util0-dev libxcb-icccm4-dev libyajl-dev libstartup-notification0-dev libxcb-randr0-dev libev-dev libxcb-cursor-dev libxcb-xinerama0-dev libxcb-xkb-dev libxkbcommon-dev libxkbcommon-x11-dev autoconf libxcb-xrm0 libxcb-xrm-dev automake
    cd /opt
    git clone https://www.github.com/Airblader/i3 i3-gaps
    cd i3-gaps    
    autoreconf --force --install
    rm -rf build/
    mkdir -p build && cd build/    
    ../configure --prefix=/usr --sysconfdir=/etc --disable-sanitizers
    make
    sudo make install

    #Theme
    sudo add-apt-repository -y ppa:system76/pop
    sudo apt update
    sudo apt -y install pop-theme

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

  # Dotfiles
  config.vm.provision :shell, inline: "mkdir -p /home/#{user}/.config/i3" 
  config.vm.provision :shell, inline: "cp /opt/i3-config /home/#{user}/.config/i3/config" 
  config.vm.provision :shell, inline: "mkdir -p /home/#{user}/.config/gtk-3.0/" 
  config.vm.provision :shell, inline: "cp /opt/gtk.css /home/#{user}/.config/gtk-3.0/gtk.css" 
  config.vm.provision :shell, inline: "cp /opt/gtk-settings.ini /home/#{user}/.config/gtk-3.0/settings.ini" 
  config.vm.provision :shell, inline: "wget https://gist.githubusercontent.com/ultrabug/37614f696cc7e4ca027f/raw/1c41f0c0a61e807df3afdef0e463949133c64659/i3status.conf -P /home/#{user}/" 
  config.vm.provision :shell, inline: "mv /home/#{user}/i3status.conf /home/#{user}/.i3status.conf" 

  

  # Default JDK is 8
  config.vm.provision :shell, inline: "echo 'export PATH=/opt/jdk-11.0.4+11/bin:\$PATH' >> /home/#{user}/.bashrc" 

  config.vm.provision :shell, inline: "echo 'export PATH=/opt/eclipse:\$PATH' >> /home/#{user}/.bashrc"  
  config.vm.provision :shell, inline: "echo 'screenfetch' >> /home/#{user}/.bashrc" 
  config.vm.provision :shell, inline: "echo 'setxkbmap ch' >> /home/#{user}/.bashrc"     
  config.vm.provision :shell, inline: "echo 'set PATH=$PATH:/opt/i3-gaps/build' >> /home/#{user}/.bashrc"       
  config.vm.provision :shell, inline: "chown -R #{user}:#{user} /opt/*"   

  # remove vagrant user
  config.vm.provision :shell, inline: "reboot"  
end
