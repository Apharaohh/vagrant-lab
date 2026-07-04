Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: "echo Hello"
  (1..3).each do |i|
    config.vm.define "node-#{i}" do |lv|
      lv.vm.box = "generic/rocky9"
      config.vm.provider "libvirt" do |lv|
        lv.memory = "1024"
        lv.cpus = 2
      end
    end
  end
end
