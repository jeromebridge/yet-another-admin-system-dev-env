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
    * <del>m2e</del>
    * <del>m2e AspectJ Connector</del>
    * <del>BndTools</del>

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
* <del>Yaas CLI</del> <sup>* Run the CLI through the Docker image</sup>
* Kdenlive (for video editing)?
* <del>Drone CLI</del>
* <del>Graylog</del> Lilith (http://lilith.huxhorn.de/)
* <del>Gradle? (I may not because I kind of like the idea of each project bootstraping their own Gradle version)</del>
* <del>JPM4J</del>
* <del>BND</del>
* <del>JDeploy (NPM)</del>
* <del>OSGi Deployer CLI</del>
* <del>Codeship Jet</del>
* <del>VLC</del>

## Debugging (Puppet)
```
cd /vagrant/puppet
sudo puppet apply manifests/default.pp --debug --modulepath=/etc/puppet/modules:modules --hiera_config=hiera.yaml --ordering=manifest
```

## Screencast (to GIF)
http://askubuntu.com/questions/107726/how-to-create-animated-gif-images-of-a-screencast
```
rm screencast.ogv; rm -Rf screencast
recordmydesktop --output=screencast.ogv
mplayer -ao null screencast.ogv -vo jpeg:outdir=screencast
convert screencast/* screencast.gif
rm screencast.ogv; rm -Rf screencast
```
OR
`byzanz-record --duration=15 screencast.gif`
`byzanz-record --duration=15 --x=200 --y=300 --width=700 --height=400 screencast.gif`


## Todo
1. <del>Setup Maven `settings.xml`</del>
2. <del>Provision Docker db</del>
5. <del>Setup Database connections in Squirrel (with needed driver(s))</del>
3. setup eclipse workspace
    6. Setup Eclipse Preferences
    7. Need to have the git user somewhere when they setup through vagrant
4. Get Repositories?
5. <del>Install Atom</del>

## Setup
1. Install libraries to workaround Vargrant issues: `sudo apt install zlib1g-dev`
2. Install Vagrant and Virtual Box: `sudo apt-get -y install vagrant virtualbox`
3. Install Git `sudo apt-get install git`
4. clone the vagrant repo: `git clone https://github.com/jeromebridge/yet-another-admin-system-dev-env`
5. change directory into the cloned repository: `cd yet-another-admin-system-dev-env`
6. Start Vagrant: `vagrant up`
7. After Vagrant has completed you should see a VirtualBox window open. Navigate to it and login using the credentials `vagrant / vagrant`

### Workarounds
1. Install missing Atom plugins (Issues #5, #17)
```
apm install language-puppet linter linter-eslint node-debugger mocha-test-runner react xml-formatter eclipse-keybindings split-diff to-base64
```
2. Add user to Docker group (Issue #16)
```
sudo usermod -aG docker $USER
```
3. Increase Max Number Of Files That Can Be Monitored (Issue #11)
```
echo fs.inotify.max_user_watches=582222 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```
4. Reload vagrant (Issue #11) -- from host machine
```
vagrant reload
```
5. Eclipse settings (Issues #23, #13)
    
    Update the `/opt/eclipse/eclipse.ini` file based on the listed issues above.

### UI Development
1. Run the Yaas backend Service: See `Docker Only` section
2. Clone the UI project: `git clone https://github.com/jeromebridge/yet-another-admin-system-web.git`
3. Change directory to project: `cd yet-another-admin-system-web`
4. Install UI Dependencies: `npm install`
5. Run the UI: `export INTEGRATED=true; export YAASURL=http://localhost:8081; npm run dev`
6. Setup the Editor. Open `Atom`
7. Click `File -> Add Project Folder...`
8. Select the folder you downloaded from GitHub: `yet-another-admin-system-web`

#### Restart Backend Service
1. Check if backend is already running on docker: `sudo docker ps -a`
2. If it is running then stop and remove the app: `sudo docker stop app; sudo docker rm app`
3. Start the service again: `sudo docker run -d --name=app --link db:db -p 8081:8080 -v /tmp/yaas:/working jeromebridge/yet-another-admin-system`
4. Tail the logs of the backend: `sudo docker logs -f app`

##### Quick One Line Command
````
sudo docker start db; sudo docker stop app; sudo docker rm app; sudo docker run -d --name=app --link db:db -p 8081:8080 -v /tmp/yaas:/working jeromebridge/yet-another-admin-system; sudo docker logs -f app
````

#### Docker Database Is Down
Sometimes the Database docker app goes down. In this case you have to tell docker to start it back up again.
1. Check the status of the running containers in docker: `sudo docker ps -a`
2. If the database status is "Exited" then you will need to start it again: `sudo docker start db`

### Backend Development
1. Clone the Backend project: `git clone https://github.com/jeromebridge/yet-another-admin-system.git`
2. Change directory to the project: `cd yet-another-admin-system`
3. Maven build: `mvn clean install`
4. Open Eclipse
    1. Switch to `Git` perspective
    2. Add an existing local Git Repository; select the `yet-another-admin-system` directory you cloned earlier
    3. Right click the `Working Directory` and select `Import Projects...`
    4. Check `Import Existing Projects`
    5. Click `Next`
    6. Uncheck `hello-maven-plugin`, `yaas-dto-generator`, `yaas-codegen-plugin`, `workflow`, `workflow api`, `yaas-ws`, `ws client` and click Finish
    7. Go back to `Java` perspective
    8. Right click on of the projects and select `Maven Update...`
    9. Click `Select All` and then `OK`
    10. Eclipse will show some errors and may even give a popup. Delete the errors from the `Problems` view and restart Eclipse
    11. `Clean all` projects when Eclipse comes back
    12. `Refresh` all the projects afer clean all completes
5. Find the TestDev file.  Right click > Run As... > Test NG Test
6. Wait a few moments for it to boot and then Open another Terminal
7. Connect to the application server: `rm -Rf ~/.ssh/known_hosts; ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 -oHostKeyAlgorithms=+ssh-dss admin@localhost -p2502`
8. Password: `admin`
9. Deploy the code from your Eclipse environment (Use the path you cloned the `yet-another-admin-system` project to): `m2e:deploy -r -f /home/vagrant/yet-another-admin-system`  (For Subsequent Deploys you should do a partial deployment: `yet-another-admin-system` project to): `m2e:deploy -r /home/vagrant/yet-another-admin-system`)

### Docker Only
#### Setup AWS
1. Create a directory `~/.aws`
2. Create a file `~/.aws/credentials`
```
[default]
aws_access_key_id=XXX
aws_secret_access_key=XXX
```
3. Create a file `~/.aws/config`
```
[default]
region=us-east-1
output=json
```
4. Initialize Docker
```shell
eval "$(docker run --rm --volume ~/.aws:/root/.aws cgswong/aws:latest aws ecr get-login | gawk '{gsub(" -e none|Id$","",$0);print}')"
```
5. Stop All
```shell
docker stop front; docker rm front; docker stop app; docker rm app; docker stop db; docker rm db;
```
5. Start Containers
```shell
docker stop db; docker rm db;
docker pull 168745904620.dkr.ecr.us-east-1.amazonaws.com/postgresql:develop
docker run -d --name=db -p 5432:5432 -e POSTGRES_USER="super" -e POSTGRES_DB="yaas" -e POSTGRES_PASSWORD="postgres" 168745904620.dkr.ecr.us-east-1.amazonaws.com/postgresql:develop
docker stop app; docker rm app;
docker pull 168745904620.dkr.ecr.us-east-1.amazonaws.com/yet-another-admin-system:develop
docker run -d --name=app --link db:db -p 8081:8080 -p 8001:8000 -p 21099:11099 168745904620.dkr.ecr.us-east-1.amazonaws.com/yet-another-admin-system:develop
docker stop front; docker rm front;
docker pull 168745904620.dkr.ecr.us-east-1.amazonaws.com/yet-another-admin-system-web:develop
docker run -d --name=front --link app:app -p 8082:8080 168745904620.dkr.ecr.us-east-1.amazonaws.com/yet-another-admin-system-web:develop
```
6. Run CLI
```shell
docker pull 168745904620.dkr.ecr.us-east-1.amazonaws.com/yet-another-admin-system-cli:develop
docker run --rm 168745904620.dkr.ecr.us-east-1.amazonaws.com/yet-another-admin-system-cli:develop
```

### Known Issues
#### 1. The configured module path doesn't exist: /home/user1/yet-another-admin-system-dev-env/puppet/modules
The is a bug in the source code that does not create the modules folder under the puppet folder.
##### Workaround
1. Create the directory: `mkdir puppet/modules`
2. Rerun vagrant: `vagrant up`

#### 2. Unknown configuration section 'librarian_puppet'.
````
user1@user1-Serval-WS:~/yet-another-admin-system-dev-env$ vagrant up
Installing the 'vagrant-hostsupdater' plugin. This can take a few minutes...
Installed the plugin 'vagrant-hostsupdater (1.0.2)'!
Installing the 'vagrant-librarian-puppet' plugin. This can take a few minutes...
Installed the plugin 'vagrant-librarian-puppet (0.9.2)'!
Bringing machine 'default' up with 'virtualbox' provider...
There are errors in the configuration of this machine. Please fix
the following errors and try again:

Vagrant:
Unknown configuration section 'librarian_puppet'.
````
##### Workaround
1. Rerun the `vagrant up` command again.

##### Possible Fix
I may have found a workaround to this issue. Vagrant needs to be restarted if the plugin is installed.
https://github.com/voxpupuli/vagrant-librarian-puppet/issues/6

#### 3. Vagrant Finishes With Non-Zero Return
````
The SSH command responded with a non-zero exit status. Vagrant
assumes that this means the command failed. The output for this command
should be in the log above. Please read the output to determine what
went wrong.
````
This happens because some of the sequence that vagrant runs is not correct.
##### Workaround
1. Rerun the Vagrant configuration only: `vagrant provision`


#### 4. The box 'box-cutter/ubuntu1404-desktop' could not be found.
You are likely using an older version of vagrant (1.4.x), which can't download the box properly.
##### Workaround
1. Download Vagrant 1.8.1 package from `https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.deb`
2. CD to the download location
3. `sudo dpkg -i vagrant_1.8.1_x86_64.deb`
4. Rerun the `vagrant up` command again.

#### 5. Cannot find module ... (any module)
You get this error when starting the UI
##### Workaround
1. CD to `/vagrant/yet-another-admin-system-web`
2. Run `npm install`

#### 6. Could not locate the bindings file
You get this error when starting the UI
##### Workaround
1. Delete everything from `/vagrant/yet-another-admin-system-web/node_modules` folder
2. CD to `/vagrant/yet-another-admin-system-web`
3. Run `npm install`

#### 7. Error Starting Vagrant:
```
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
```
A few problems here:

1. Install `ruby-dev`
    ```
    sudo apt-get install ruby-dev
    ```
2. Remove vagrant cached plugins
    ```
    rm -Rf ~/.vagrant.d
    ```
3. You must also download and install vagrant version 1.7.4 from the web site.  (no automatic update available)
4. Upgrade Ruby to version above 2
    ```
    sudo apt-get update
    sudo apt-get install build-essential make curl
    gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
    \curl -sSL https://get.rvm.io | bash -s stable
    source ~/.bash_profile
    rvm install ruby-2.1.4

    rvm list
    rvm use --default ruby-2.1.4
    ```
    Check http://stackoverflow.com/questions/26595620/how-to-install-ruby-2-1-4-on-ubuntu-14-04 for more details on the instructions above.

#### 8. Error running vagrant up

##### Workaround
You can find a patch that must be applied to version 1.8.1 of vagrant:
http://stackoverflow.com/questions/36811863/cant-install-vagrant-plugins-in-ubuntu-16-04/36991648#36991648

#### 9. Error Starting Vagrant: undefined method `plugins_enabled?'

See #24 for details. You may just need to update your system. `sudo apt-get update && sudo apt-get upgrade -y`


## Notes
https://blog.jessfraz.com/post/docker-containers-on-the-desktop/
