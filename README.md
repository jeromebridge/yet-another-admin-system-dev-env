# yet-another-admin-system-dev-env
Vagrant configuraiton for Yet Another Admin System development environment.

## Tools
* <del>Eclipse (with plugins)</del>
    **Plugins**
    * <del>OSGi Integration Plugin: http://repo1.maven.org/maven2/.m2e/connectors/m2eclipse-tycho/0.7.0/N/0.7.0.201309291400/</del>
    * <del>AspectJ Development Tools: http://download.eclipse.org/tools/ajdt/44/dev/update/</del>
    * <del>AspectJ M2E Configurer: http://dist.springsource.org/release/AJDT/configurator/</del>
    * <del>TestNG Plugin: http://beust.com/eclipse</del>
    * <del>Java Decompiler Plugin: Special instructions</del>
    * <del>buildhelper Maven Connector</del>
    * <del>m2e-apt Maven Connector</del>
    * m2e
    * m2e AspectJ Connector

* <del>VisualVM</del>
* <del>Java Decompiler</del>
* <del>Git</del>
* <del>Docker</del>
* <del>Squirrel SQL (setup for database)</del>
* <del>Maven</del>
* <del>JDK(s)</del>
* <del>Brackets Editor</del>
* <del>NodeJS</del>
* <del>Atom</del>
    * <del>Node Debugger</del>
* <del>Ruby (in order to run `rultor` command)</del>
* <del>Rultor (Ruby Gem)</del>
* <del>gnugp2 (Public/Private Keys) ; Force alias to gpg2 so rultor works: https://wiki.debian.org/Teams/GnuPG/UsingGnuPGv2</del>
* Yaas CLI
* Kdenlive (for video editing)?
* <del>Graylog</del> Lilith (http://lilith.huxhorn.de/)
* <del>Gradle? (I may not because I kind of like the idea of each project bootstraping their own Gradle version)</del>


## Debugging (Puppet)
````
cd /vagrant/puppet
sudo puppet apply manifests/default.pp --debug --modulepath=/etc/puppet/modules:modules --hiera_config=hiera.yaml --ordering=manifest
````

## Todo
1. <del>Setup Maven `settings.xml`</del>
2. <del>Provision Docker db</del>
5. <del>Setup Database connections in Squirrel (with needed driver(s))</del>
3. setup eclipse workspace
    6. Setup Eclipse Preferences
    7. Need to have the git user somewhere when they setup through vagrant
4. Get Repositories?
5. <del>Install Atom</del>

## FAQ
# Error Starting Vagrant
````
paul@PAULS:~/workspaces/Yaas/yet-another-admin-system-dev-env$ vagrant upInstalling the 'vagrant-librarian-puppet' plugin. This can take a few minutes...
/usr/lib/ruby/1.9.1/rubygems/installer.rb:562:in `rescue in block in build_extensions': ERROR: Failed to build gem native extension. (Gem::Installer::ExtensionBuildError)

        /usr/bin/ruby1.9.1 extconf.rb
/usr/lib/ruby/1.9.1/rubygems/custom_require.rb:36:in `require': cannot load such file -- mkmf (LoadError)
	from /usr/lib/ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
	from extconf.rb:1:in `<main>'


Gem files will remain installed in /home/paul/.vagrant.d/gems/gems/json-1.8.3 for inspection.
Results logged to /home/paul/.vagrant.d/gems/gems/json-1.8.3/ext/json/ext/generator/gem_make.out
	from /usr/lib/ruby/1.9.1/rubygems/installer.rb:540:in `block in build_extensions'
	from /usr/lib/ruby/1.9.1/rubygems/installer.rb:515:in `each'
	from /usr/lib/ruby/1.9.1/rubygems/installer.rb:515:in `build_extensions'
	from /usr/lib/ruby/1.9.1/rubygems/installer.rb:180:in `install'
	from /usr/lib/ruby/1.9.1/rubygems/dependency_installer.rb:297:in `block in install'
	from /usr/lib/ruby/1.9.1/rubygems/dependency_installer.rb:270:in `each'
	from /usr/lib/ruby/1.9.1/rubygems/dependency_installer.rb:270:in `each_with_index'
	from /usr/lib/ruby/1.9.1/rubygems/dependency_installer.rb:270:in `install'
	from /usr/share/vagrant/plugins/commands/plugin/action/install_gem.rb:65:in `block in call'
	from /usr/share/vagrant/plugins/commands/plugin/gem_helper.rb:42:in `block in with_environment'
	from /usr/lib/ruby/1.9.1/rubygems/user_interaction.rb:40:in `use_ui'
	from /usr/share/vagrant/plugins/commands/plugin/gem_helper.rb:41:in `with_environment'
	from /usr/share/vagrant/plugins/commands/plugin/action/install_gem.rb:52:in `call'
	from /usr/lib/ruby/vendor_ruby/vagrant/action/warden.rb:34:in `call'
	from /usr/share/vagrant/plugins/commands/plugin/action/bundler_check.rb:20:in `call'
	from /usr/lib/ruby/vendor_ruby/vagrant/action/warden.rb:34:in `call'
	from /usr/lib/ruby/vendor_ruby/vagrant/action/builder.rb:116:in `call'
	from /usr/lib/ruby/vendor_ruby/vagrant/action/runner.rb:69:in `block in run'
	from /usr/lib/ruby/vendor_ruby/vagrant/util/busy.rb:19:in `busy'
	from /usr/lib/ruby/vendor_ruby/vagrant/action/runner.rb:69:in `run'
	from /usr/share/vagrant/plugins/commands/plugin/command/base.rb:17:in `action'
	from /usr/share/vagrant/plugins/commands/plugin/command/install.rb:27:in `execute'
	from /usr/share/vagrant/plugins/commands/plugin/command/root.rb:56:in `execute'
	from /usr/lib/ruby/vendor_ruby/vagrant/cli.rb:38:in `execute'
	from /usr/lib/ruby/vendor_ruby/vagrant/environment.rb:484:in `cli'
	from /usr/bin/vagrant:127:in `<main>'
Bringing machine 'default' up with 'virtualbox' provider...
There are errors in the configuration of this machine. Please fix
the following errors and try again:

vm:
* The box 'box-cutter/ubuntu1404-desktop' could not be found.

Vagrant:
* Unknown configuration section 'librarian_puppet'.
````
A few problems here:

1. Install `ruby-dev`
    ````
    sudo apt-get install ruby-dev
    ````
2. Remove vagrant cached plugins
    ````
    rm -Rf ~/.vagrant.d
    ````
3. You must also download and install vagrant version 1.7 or higher from the web site.  (no automatic update available)
