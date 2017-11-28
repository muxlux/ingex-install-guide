# ingex-install-guide

## assumes:

1. installation of opensuse 13.2 w/
   - Server
     - File server
     - Web and LAMP server
     - C/C++ development
     - Perl development 
     - Linux Kernel Development
   - Security
     - firewall disabled
     - ssh enabled
     
2. ATI graphics card
3. raid0 mounted at /video

## get source 

```
mkdir ~/ap-workspace
cd ~/ap-workspace
cvs -d:pserver:anonymous@ingex.cvs.sourceforge.net:/cvsroot/ingex login
cvs -z3 -d:pserver:anonymous@ingex.cvs.sourceforge.net:/cvsroot/ingex co -P ingex
export workspace=/home/ingex/ap-workspace/ingex
echo "export workspace=/home/ingex/ap-workspace/ingex" >> ~/.bashrc
```
## get & install prerequisites
```
sudo zypper in libjpeg62-devel libbz2-devel portaudio-devel postgresql-server postgresql-devel libpqxx-devel libXerces-c-devel wxWidgets-devel libSDL-devel perl-CGI-Session perl-Clone perl-common-sense perl-DBD-Pg perl-DBI perl-JSON-XS perl-Linux-Inotify2 perl-Log-Dispatch perl-Log-Log4perl perl-Switch perl-Template-Toolkit perl-Text-Template perl-XML-Simple
sudo cpan Filesys::DfPortable IPC::ShareLite Log::Handler PDF::Create Proc::Daemon Term::ANSIColor
mkdir ~/rpms
cd ~/rpms
firefox -preferences #make ~/rpms the Download directory
firefox http://download.opensuse.org/repositories/devel:/libraries:/ACE:/micro/openSUSE_13.2/x86_64/ace-6.4.5-66.x86_64.rpm
firefox http://download.opensuse.org/repositories/devel:/libraries:/ACE:/micro/openSUSE_13.2/x86_64/ace-devel-6.4.5-66.x86_64.rpm
firefox http://download.opensuse.org/repositories/devel:/libraries:/ACE:/micro/openSUSE_13.2/x86_64/ace-gperf-6.4.5-66.x86_64.rpm
firefox http://download.opensuse.org/repositories/devel:/libraries:/ACE:/micro/openSUSE_13.2/x86_64/ace-kokyu-6.4.5-66.x86_64.rpm
firefox http://download.opensuse.org/repositories/devel:/libraries:/ACE:/micro/openSUSE_13.2/x86_64/ace-xml-6.4.5-66.x86_64.rpm
firefox http://download.opensuse.org/repositories/devel:/libraries:/ACE:/micro/openSUSE_13.2/x86_64/mpc-6.4.5-66.x86_64.rpm
firefox http://download.opensuse.org/repositories/devel:/libraries:/ACE:/micro/openSUSE_13.2/x86_64/tao-2.4.5-66.x86_64.rpm
firefox http://download.opensuse.org/repositories/devel:/libraries:/ACE:/micro/openSUSE_13.2/x86_64/tao-cosnaming-2.4.5-66.x86_64.rpm
firefox http://download.opensuse.org/repositories/devel:/libraries:/ACE:/micro/openSUSE_13.2/x86_64/tao-devel-2.4.5-66.x86_64.rpm
wget https://sourceforge.net/projects/ingex/files/1.0.0/prerequisites/opensuse_11.4_x86_64/codecs-for-ffmpeg-20081215-2.x86_64.rpm/download
wget https://sourceforge.net/projects/ingex/files/1.0.0/prerequisites/opensuse_11.4_x86_64/ffmpeg-DNxHD-h264-aac-0.5-11.x86_64.rpm/download
wget https://sourceforge.net/projects/ingex/files/1.0.0/prerequisites/opensuse_11.4_x86_64/ffmpeg-DNxHD-h264-aac-0.5-11.x86_64.rpm/download
sudo zypper in openssl-devel
sudo rpm -i *
firefox -preferences #remake ~/Downloads the Download directory
```
<log out & back in>
Download Blackmagic Desktop Video and Decklink SDK from https://www.blackmagicdesign.com/support/family/capture-and-playback
```
tar xf /home/ingex/Downloads/Blackmagic_Desktop_Video_Linux*
tar xf /home/ingex/Downloads/Blackmagic_Decklink* -C /home/ingex
export BMD_HARDWARE_INCLUDE=/home/ingex/BlackmagicDeckLinkSDK/Linux/include
sudo zypper ar -f -n packman http://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_13.2/ packman
sudo zypper refresh
sudo zypper in dkms
mkdir ~/AAFtoolkit
cd ~/AAFtoolkit
cvs -d:pserver:anonymous@aaf.cvs.sourceforge.net:/cvsroot/aaf co -P AAF
cd AAF
make install DISABLE_FFMPEG=1
export AAFSDKINSTALL=/home/ingex/AAFtoolkit/AAF/AAFx86_64LinuxSDK/g++
echo export AAFSDKINSTALL=/home/ingex/AAFtoolkit/AAF/AAFx86_64LinuxSDK/g++ >> ~/.bashrc
mkdir bmx
cd bmx
git clone http://git.code.sf.net/p/bmxlib/libmxf libMXF
git clone http://git.code.sf.net/p/bmxlib/libmxfpp libMXF++
git clone http://git.code.sf.net/p/bmxlib/bmx bmx
cd libMXF
./autogen.sh && ./configure && make && make check && sudo make install
cd ../libMXF++
./autogen.sh && ./configure && make && make check && sudo make install
cd ../bmx
./autogen.sh && ./configure && make && make check && sudo make install
export webroot=/srv/www
wget https://github.com/probonogeek/extjs/archive/4.1.1a.zip
unzip extjs-4.1.1a.zip 
sudo mkdir $webroot/htdocs/ingex
sudo mv extjs-4.1.1a $webroot/htdocs/ingex/ext
mkdir /tmp/flowplayer
cd /tmp/flowplayer/
wget http://releases.flowplayer.org/flowplayer/flowplayer-3.2.15.zip
unzip flowplayer-3.2.15.zip
cd flowplayer
wget http://releases.flowplayer.org/flowplayer.content/flowplayer.content-3.2.8.swf 
wget http://releases.flowplayer.org/flowplayer.pseudostreaming/flowplayer.pseudostreaming-3.2.11.swf
wget http://releases.flowplayer.org/flowplayer.audio/flowplayer.audio-3.2.10.swf
sudo mkdir -p $webroot/htdocs/ingex/review/lib/flowplayer
sudo mv flowplayer*.min.js $webroot/htdocs/ingex/review/lib/flowplayer/flowplayer.min.js
sudo mv flowplayer-*.swf $webroot/htdocs/ingex/review/lib/flowplayer/flowplayer.swf
sudo mv flowplayer.audio*.swf $webroot/htdocs/ingex/review/lib/flowplayer/flowplayer.audio.swf
sudo mv flowplayer.content*.swf $webroot/htdocs/ingex/review/lib/flowplayer/flowplayer.content.swf
sudo mv flowplayer.controls*.swf $webroot/htdocs/ingex/review/lib/flowplayer/flowplayer.controls.swf
sudo mv flowplayer.pseudostreaming*.swf $webroot/htdocs/ingex/review/lib/flowplayer/flowplayer.pseudostreaming.swf
cd
rm -r /tmp/flowplayer
echo $ACE_ROOT
echo $AAFSDKINSTALL
cd $workspace/YUVlib
make
sudo make install
cd $workspace
sudo zypper in  libxtst-devel libxv-devel
make
```
:)
```  
cd $workspace/player/ingex_player
sudo make install
cd $workspace/studio/ace-tao/Ingexgui
sudo make install
```

