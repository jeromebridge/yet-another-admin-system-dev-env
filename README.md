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

## Screencast (to GIF)
http://askubuntu.com/questions/107726/how-to-create-animated-gif-images-of-a-screencast

````
rm screencast.ogv; rm -Rf screencast
recordmydesktop --output=screencast.ogv
mplayer -ao null screencast.ogv -vo jpeg:outdir=screencast
convert screencast/* screencast.gif
rm screencast.ogv; rm -Rf screencast
````

OR
`byzanz-record --duration=15 screencast.gif`

`byzanz-record --duration=15 --x=200 --y=300 --width=700 --height=400 screencast.gif`

## Yaas Setup
1. Install libraries to workaround Vargrant issues: `sudo apt install zlib1g-dev`
2. Install Vagrant and Virtual Box: `sudo apt-get -y install vagrant virtualbox`
3. clone the vagrant repo: `git clone https://github.com/jeromebridge/yet-another-admin-system-dev-env`
4. change directory into the cloned repository: `cd yet-another-admin-system-dev-env`
5. Start Vagrant: `vagrant up`
6. After Vagrant has completed you should see a VirtualBox window open. Navigate to it and login using the credentials `vagrant / vagrant`

### UI Development
1. Run the Yaas backend Service:
    1. Make temp directory for Yaas: `mkdir /tmp/yaas`
    2. Download and Run Yaas Docker image: `sudo docker run -d --name=app --link db:db -p 8081:8080 -v /tmp/yaas:/working jeromebridge/yet-another-admin-system`
2. Clone the UI project: `git clone https://github.com/jeromebridge/yet-another-admin-system-web.git`
3. Change directory to project: `cd yet-another-admin-system-web`
4. Install UI Dependencies: `npm install`
5. Run the UI: `export INTEGRATED=true; export YAASURL=http://localhost:8080; npm run dev`

#### Restart Backend Service
1. Check if backend is already running on docker: `sudo docker ps -a`
2. If it is running then stop and remove the app: `sudo docker stop app; sudo docker rm app`
3. Start the service again: `sudo docker run -d --name=app --link db:db -p 8081:8080 -v /tmp/yaas:/working jeromebridge/yet-another-admin-system`
4. Tail the logs of the backend: `sudo docker logs -f app`

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
    6. Uncheck `hello-maven-plugin`, `yaas-dto-generator`, `yaas-codegen-plugin` and click Finish
    7. Go back to `Java` perspective
    8. Right click on of the projects and select `Maven Update...`
    9. Click `Select All` and then `OK`
    10. Eclipse will show some errors and may even give a popup. Delete the errors from the `Problems` view and restart Eclipse
    11. `Clean all` projects when Eclipse comes back
    12. `Refresh` all the projects afer clean all completes
5. Open Terminal again
6. Clone the Boot project: `git clone https://github.com/jeromebridge/yet-another-admin-system-boot.git`
7. Change directory to project: `cd yet-another-admin-system-boot`
8. Run application server: `./gradlew setupVirgoNoYaas runVirgo`
9. Open a New Terminal window
10. Connect to the application server: `rm -Rf ~/.ssh/known_hosts; ssh admin@localhost -p2502`
11. Password: `springsource`
12. Deploy the code from your Eclipse environment (Use the path you cloned the `yet-another-admin-system` project to): `m2e:deploy -r /home/vagrant/git/yet-another-admin-system`

### Known Issues
#### The configured module path doesn't exist: /home/user1/yet-another-admin-system-dev-env/puppet/modules
The is a bug in the source code that does not create the modules folder under the puppet folder.
##### Workaround
1. Create the directory: `mkdir puppet/modules`
2. Rerun vagrant: `vagrant up`

#### Unknown configuration section 'librarian_puppet'.
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

#### Vagrant Finishes With Non-Zero Return
````
The SSH command responded with a non-zero exit status. Vagrant
assumes that this means the command failed. The output for this command
should be in the log above. Please read the output to determine what
went wrong.
````
This happens because some of the sequence that vagrant runs is not correct.
##### Workaround
1. Rerun the Vagrant configuration only: `vagrant provision`

#### The box 'box-cutter/ubuntu1404-desktop' could not be found.
You are likely using an older version of vagrant (1.4.x), which can't download the box properly.
##### Workaround
1. Download Vagrant 1.8.1 package from `https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.deb`
2. CD to the download location
3. `sudo dpkg -i vagrant_1.8.1_x86_64.deb`
4. Rerun the `vagrant up` command again.
