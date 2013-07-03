# -*- mode: ruby -*-
# vi: set ft=ruby sw=2 ts=2 :

Vagrant::Config.run do |config|

  config.ssh.timeout = 60

  # Define a Ubuntu Server image (cloud) for the 12.04 release (precise)
  config.vm.define :precise do |precise_config|
    precise_config.vm.box = "precise-cloud-amd64"
    precise_config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"
  end

  # Define a Ubuntu Server image (cloud) for the 12.10 release (quantal)
  #config.vm.define :quantal do |quantal_config|
  #  quantal_config.vm.box = "quantal-cloud-amd64"
  #  quantal_config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/quantal/current/quantal-server-cloudimg-amd64-vagrant-disk1.box"
  #end

  # Define a Ubuntu Server image (cloud) for the 13.04 release (raring)
  config.vm.define :raring do |raring_config|
    raring_config.vm.box = "raring-cloud-amd64"
    raring_config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/raring/current/raring-server-cloudimg-amd64-vagrant-disk1.box"
  end

  # For debugging and later future GUI testing
  if ENV.key? "VAGRANT_GUI"
    config.vm.boot_mode = :gui
  end

  # Setup an apt cache if one is available
  if ENV.key? "VAGRANT_APT_CACHE"
    config.vm.provision :shell, :inline => "echo 'Acquire::http { Proxy \"#{ENV['VAGRANT_APT_CACHE']}\"; };' > /etc/apt/apt.conf"
  end

  # Update to have the latest packages, this is needed because the image comes
  # with an old (and no longer working) apt cache and links to many packages no
  # longer work.
  config.vm.provision :shell, :inline => "apt-get update && DEBIAN_FRONTEND=noninteractive apt-get dist-upgrade --yes"
  # Install dependencies from native packages
  config.vm.provision :shell, :inline => "apt-get install --yes ubuntu-desktop"
  # Auto-login the vagrant user
  config.vm.provision :shell, :inline => "echo 'autologin-user=vagrant' >> /etc/lightdm/lightdm.conf"
  # Purge the apt cache to make the image smaller
  config.vm.provision :shell, :inline => "apt-get clean"

  # De-configure the apt cache if one was created before
  # This is important because we need to package the image afterwards
  if ENV.key? "VAGRANT_APT_CACHE"
    config.vm.provision :shell, :inline => "rm -f /etc/apt/apt.conf"
  end
end
