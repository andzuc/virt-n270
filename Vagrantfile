title = 'debian-testapp'

machines = {
  "main" => {
    "name" => "testapp" ,
    "ip" => "172.30.1.2",
    "netmask" => "255.255.255.0"
  }
}

Vagrant.configure("2") do |config|    
  config.vm.define :main do |main|
    machine = machines["main"]
    main.vm.hostname = machine["name"]
    main.vm.synced_folder '.', '/vagrant', disabled: true
    main.vm.network :public_network,
                    :ip => machine["ip"],
                    :netmask => machine["netmask"],
                    :dev => "intnet",
                    :mode => "bridge",
                    :type => "bridge"
    
    main.vm.provider :libvirt do |libvirt|
      libvirt.memory = 1024
      libvirt.cpus = 2
      libvirt.cpu_mode = "custom"
      libvirt.cpu_model = "n270"
      libvirt.cputopology sockets: "1", cores: "1", threads: "2"
      libvirt.nested = false
      libvirt.machine_arch = "i686"
      libvirt.title = title
      libvirt.machine_type = 'pc-i440fx-5.2'
      libvirt.boot 'cdrom'
      
      # Network
      libvirt.nic_model_type = "e1000"
      
      # USB
      # libvirt.usb_controller :model => 'qemu-xhci'
      # libvirt.usb :vendor => '0x174c', :product => '0x1153', :startupPolicy => 'mandatory'
      
      # Configurazione grafica
      libvirt.graphics_type = 'spice'
      libvirt.graphics_gl = false
      libvirt.video_type = 'virtio'
      libvirt.video_accel3d = false
      libvirt.channel :type => 'unix', :target_name => 'org.qemu.guest_agent.0', :target_type => 'virtio'
      libvirt.channel :type => 'spicevmc', :target_name => 'com.redhat.spice.0', :target_type => 'virtio'

      libvirt.storage :file, 
                      :device => :cdrom, 
                      :bus => 'ide',
                      :path => '/media/tera/zakcloud/isoz/Debian/debian-31r0a-i386-businesscard.iso'
    end
  end
end
