Vagrant.configure("2") do |config|
  
  config.vm.box = "xxxxxxxxxxxxxxxxx"

  config.vm.provider :aws do |aws, override|
    aws.access_key_id = "xxxxxxxxxxxxxxxxx"
    aws.secret_access_key = "xxxxxxxxxxxxxxxxx"
    aws.keypair_name = "xxxxxxxxxxxxxxxxx"

    aws.ami = "ami-7747d01e"

    aws.security_groups = [ 'vagrant' ]

    override.ssh.username = "ubuntu"
    override.ssh.private_key_path = "xxxxxxxxxxxxxxxxx"
  end

  
  $provision_script= <<SCRIPT
    if [[ $(which chef-solo) != '/usr/bin/chef-solo' ]]; then
      curl -L https://www.opscode.com/chef/install.sh | sudo bash
      echo 'export PATH="/opt/chef/embedded/bin:$PATH"' >> ~/.bash_profile && source ~/.bash_profile
    fi
    mkdir /usr/local/cookbooks
SCRIPT
  
  config.vm.provision :shell, :inline => $provision_script

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks", [:vm, "/vagrant/cookbooks"]]
    chef.add_recipe :apt
    chef.add_recipe 'redis'
    chef.add_recipe 'rvm'
    chef.add_recipe 'vim'
    chef.add_recipe 'git'
    chef.add_recipe 'postgresql::server'
    chef.add_recipe 'couchdb'
    chef.add_recipe 'nodejs'
    chef.add_recipe 'mongodb::default'
    chef.add_recipe 'nginx'
    chef.json = {
      :rvm => {
        :vagrant => {
          :system_chef_client => "/opt/chef/bin/chef-client",
          :system_solo_client => "/opt/chef/bin/chef-solo"
        }
      },
      :redis      => {
        :bind        => "127.0.0.1",
        :port        => "6379",
        :config_path => "/etc/redis/redis.conf",
        :daemonize   => "yes",
        :timeout     => "300",
        :loglevel    => "notice"
      },
      :vim        => {
        :extra_packages => [
          "vim-rails"
        ]
      },
      :git        => {
        :prefix => "/usr/local"
      },
      :postgresql => {
        :config   => {
          :listen_addresses => "*",
          :port             => "5432"
        },
        :pg_hba   => [
          {
            :type   => "local",
            :db     => "postgres",
            :user   => "postgres",
            :addr   => nil,
            :method => "trust"
          },
          {
            :type   => "host",
            :db     => "all",
            :user   => "all",
            :addr   => "0.0.0.0/0",
            :method => "md5"
          },
          {
            :type   => "host",
            :db     => "all",
            :user   => "all",
            :addr   => "::1/0",
            :method => "md5"
          }
        ],
        :password => {
          :postgres => "password"
        }
      },
      :mongodb    => {
        :dbpath  => "/var/lib/mongodb",
        :logpath => "/var/log/mongodb",
        :port    => "27017"
      },
      :nginx      => {
        :dir                => "/etc/nginx",
        :log_dir            => "/var/log/nginx",
        :binary             => "/usr/sbin/nginx",
        :user               => "www-data",
        :init_style         => "runit",
        :pid                => "/var/run/nginx.pid",
        :worker_connections => "1024"
      }
    }
  end
end
