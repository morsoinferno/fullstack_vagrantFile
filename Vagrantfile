Vagrant.require_version ">= 1.5.3"

VAGRANTFILE_API_VERSION = "2"

MEMORY = 6000
CPU_COUNT = 4

# map the name of the git branch that we use for a release
# to a name and a file path, which are used for retrieving
# a Vagrant box from the internet.
openedx_releases = {
  "openedx/rc/aspen-2014-09-10" => {
    :name => "aspen-fullstack-rc1", :file => "20141010-aspen-fullstack-rc1.box",
  },
  "aspen.1" => {
    :name => "aspen-fullstack-1", :file => "20141028-aspen-fullstack-1.box",
  },
  "named-release/aspen" => {
    :name => "aspen-fullstack-1", :file => "20141028-aspen-fullstack-1.box",
  },
  "named-release/birch.rc1" => {
    :name => "birch-fullstack-rc1", :file => "20150204-birch-fullstack-rc1.box"
  },
  "named-release/birch.rc2" => {
    :name => "birch-fullstack-rc2", :file => "20150211-birch-fullstack-rc2.box"
  },
  "named-release/birch.rc3" => {
    :name => "birch-fullstack-rc3", :file => "20150213-birch-fullstack-rc3.box"
  },
  "named-release/birch" => {
    :name => "birch-fullstack", :file => "20150224-birch-fullstack.box",
  },
}
openedx_releases.default = {
  :name => "kifli-fullstack", :file => "20140826-kifli-fullstack.box"
}
rel = ENV['OPENEDX_RELEASE']

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Creates an edX fullstack VM from an official release
  config.vm.box     = openedx_releases[rel][:name]
  #config.vm.box_url = "http://files.edx.org/vagrant-images/#{openedx_releases[rel][:file]}"
  config.vm.box_url = "file://../boxes/respaldo_17_04_2015_n3_fullstack.box"

  config.vm.synced_folder  ".", "/vagrant", disabled: true
  config.ssh.insert_key = true

  #config.vm.network :private_network, ip: "192.168.33.10"
  #config.hostsupdater.aliases = ["preview.localhost"]
  config.vm.network :forwarded_port, host: 18000, guest:80

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", MEMORY.to_s]
    vb.customize ["modifyvm", :id, "--cpus", CPU_COUNT.to_s]

    # Allow DNS to work for Ubuntu 12.10 host
    # http://askubuntu.com/questions/238040/how-do-i-fix-name-service-for-vagrant-client
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  ["vmware_fusion", "vmware_workstation"].each do |vmware_provider|
    config.vm.provider vmware_provider do |v, override|
      override.vm.box     = "kifli-fullstack-vmware"
      override.vm.box_url = "http://files.edx.org/vagrant-images/20140829-kifli-fullstack-vmware.box"
      v.vmx["memsize"] = MEMORY.to_s
      v.vmx["numvcpus"] = CPU_COUNT.to_s
    end
  end
end
