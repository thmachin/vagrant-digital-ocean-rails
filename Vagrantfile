Vagrant.configure("2") do |config|
  
  config.vm.provider :digital_ocean do |provider|
    provider.client_id = ENV['DO_CLIENT_ID']
    provider.api_key = ENV['DO_API_KEY']
  end

  VAGRANT_JSON = MultiJson.load(Pathname(__FILE__).dirname.join('.', 'node.json').read)
  
  config.vm.define :web do |web|
    web.vm.box = 'precise64'
    web.vm.box_url = 'http://files.vagrantup.com/precise64.box'
    
    web.vm.network :private_network, ip: "192.168.60.10"
    web.berkshelf.enabled = true
 
    web.vm.provision :shell, :inline => "gem install chef --version 11.4.2 --no-rdoc --no-ri --conservative"
 
    web.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = ["cookbooks"]
      chef.json = VAGRANT_JSON
      chef.log_level = :info
    
      VAGRANT_JSON['run_list'].each do |recipe|
        chef.add_recipe(recipe)
      end if VAGRANT_JSON['run_list']
    end

   #  web.vm.provision :chef_solo do |chef|
   #    chef.add_recipe "apt"
   #    chef.add_recipe "build-essential"
   #    chef.add_recipe "rvm::vagrant"
   #    chef.add_recipe "rvm::system"
   #    chef.add_recipe "git"
   #    chef.add_recipe "postgresql"
   #    chef.add_recipe "nginx"

   #    chef.json.merge!({
   #       :rvm => {
   #       :default_ruby => 'ruby-2.0.0'
   #      }
   #    })
   # end

   

  end
end