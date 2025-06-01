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
      libvirt.driver = 'kvm'
      libvirt.cpu_mode = 'host-model'
      libvirt.title = title
      libvirt.memory = 2048
      libvirt.cpus = 2
      libvirt.machine_type = 'pc-i440fx-5.2'
      libvirt.boot 'hd'
      
      # USB
      libvirt.usb_controller :model => 'qemu-xhci'
      libvirt.usb :vendor => '0x174c', :product => '0x1153', :startupPolicy => 'mandatory'
      
      # Configurazione grafica
      libvirt.graphics_type = 'spice'
      libvirt.graphics_gl = false
      libvirt.video_type = 'virtio'
      libvirt.video_accel3d = false
      libvirt.channel :type => 'unix', :target_name => 'org.qemu.guest_agent.0', :target_type => 'virtio'
      libvirt.channel :type => 'spicevmc', :target_name => 'com.redhat.spice.0', :target_type => 'virtio'
    end
  end
end
