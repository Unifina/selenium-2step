# -*- mode: ruby -*-
# vi: set ft=ruby :

# Author: Yves Hwang, 03.06.2014
# http://macyves.wordpress.com

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.synced_folder ".", "/vagrant"
    config.vm.synced_folder ".", "/foo"
    config.vm.define :selenium do |selenium|
        selenium.vm.box = "ubuntu/xenial64"
#        selenium.vm.network "forwarded_port", guest: 4444, host:4444
        $script_selenium = <<SCRIPT
echo ==== Create a selenium folder ====
mkdir /usr/local/selenium
echo ==== Installing dependencies, curl, wget, unzip ====
apt-get update
apt-get install wget -y
apt-get install curl -y
apt-get install unzip -y
echo ==== Installing Java ====
apt-get install openjdk-8-jre -y 
# apt-get install openjdk-7-jdk -y
apt-get install ant -y
echo ==== Installing firefox ====
apt-get install firefox -y
echo ==== Installing chrome ====
# had problems with chromedriver 2.20 and 2.21, was 2.19
wget -nv http://chromedriver.storage.googleapis.com/2.21/chromedriver_linux64.zip
wget -nv https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
mv *.zip /usr/local/selenium
mv *.deb /usr/local/selenium
unzip /usr/local/selenium/*.zip -d /usr/local/selenium
chmod a+rx /usr/local/selenium/chromedriver
dpkg -i /usr/local/selenium/google-chrome*; sudo apt-get -f install -y
echo ==== Setting up Xvfb ====
apt-get install xvfb -y
cp /vagrant/Xvfb /etc/init.d/.
update-rc.d Xvfb defaults
service Xvfb start
echo ==== Setting up selenium ====
cp /vagrant/config.env /usr/local/selenium
source /usr/local/selenium/config.env
wget -nv -O $SELENIUM_JAR $SELENIUM_DOWNLOAD_URL
mv *.jar /usr/local/selenium/.
cp /vagrant/selenium-grid /etc/init.d/.
update-rc.d selenium-grid defaults
service selenium-grid start
cp /vagrant/selenium-node /etc/init.d/.
update-rc.d selenium-node defaults
service selenium-node start
SCRIPT
        selenium.vm.provision :shell, :inline => $script_selenium
    end
end
