# -*- mode: ruby -*-

dir = File.dirname(File.expand_path(__FILE__))

require 'yaml'
require "#{dir}/puphpet/ruby/deep_merge.rb"
require "#{dir}/puphpet/ruby/to_bool.rb"
require "#{dir}/puphpet/ruby/puppet.rb"

configValues = YAML.load_file("#{dir}/puphpet/config.yaml")

provider = ENV['VAGRANT_DEFAULT_PROVIDER'] ? ENV['VAGRANT_DEFAULT_PROVIDER'] : 'local'
if File.file?("#{dir}/puphpet/config-#{provider}.yaml")
  custom = YAML.load_file("#{dir}/puphpet/config-#{provider}.yaml")
  configValues.deep_merge!(custom)
end

if File.file?("#{dir}/puphpet/config-custom.yaml")
  custom = YAML.load_file("#{dir}/puphpet/config-custom.yaml")
  configValues.deep_merge!(custom)
end

data = configValues['vagrantfile']

Vagrant.require_version '>= 1.8.1'

Vagrant.configure('2') do |config|
  eval File.read("#{dir}/puphpet/vagrant/Vagrantfile-#{data['target']}")
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true
  config.vm.define 'wpdev' do |node|
    node.vm.hostname = 'wpdev.local'
    node.vm.network :private_network, ip: '192.168.55.22'
    node.hostmanager.aliases = %w(wpdev www.wpdev.local)
  end
  
end
