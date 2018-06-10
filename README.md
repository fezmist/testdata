# mono-5.12 offline package for centos 6.6

Install mono CentOS 6.6 offline mode:
--------------------------------------
Download Oracle VirtualBox / VMware Player and CentOS 6.6 image
VirtualBox: https://www.virtualbox.org/wiki/Downloads
VMware player: https://my.vmware.com/en/web/vmware/free#desktop_end_user_computing/vmware_workstation_player/12_0
CentOS 6.6 image: 

Launch CentOS 6.6 and map a shared folder e.g. c:/data/mono -> /mnt/apps/mono

#yum download plugin RHEL 6 required for downloading rpm packages
yum install yum-plugin-downloadonly

#add mono epel repository
rpm --import "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF"
su -c 'curl https://download.mono-project.com/repo/centos6-stable.repo | tee /etc/yum.repos.d/mono-centos6-stable.repo'

#create dir /apps/mono/packages you can give any other suitable name, in this instance we are downloading latest mono version 5.12 hence the folder name
mkdir /apps/mono/packages/mono-5.12

#download packages
yum install --downloadonly --downloaddir=/some/arbitrary/path [package]

#Note: If you ignore the --downloaddir yum will automatically download to /var/cache/yum

yum install --downloadonly --downloaddir=/apps/mono/packages/mono-5.12 mono-complete xsp referencesassemblies-pcl

#you should see similar rpm files in the /apps/mono/packages/mono-5.12 directory
cairo-1.8.8-6.el6_6.x86_64.rpm
fontconfig-2.8.0-5.el6.x86_64.rpm
freetype-2.3.11-17.el6.x86_64.rpm
giflib-4.1.6-3.1.el6.x86_64.rpm
glib2-2.28.8-9.el6.x86_64.rpm
glib2-devel-2.28.8-9.el6.x86_64.rpm
ibm-data-db2-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
libexif-0.6.21-5.el6_3.x86_64.rpm
libgdiplus-devel-5.6-0.xamarin.1.epel6.x86_64.rpm
libgdiplus0-5.6-0.xamarin.1.epel6.x86_64.rpm
libICE-1.0.6-1.el6.x86_64.rpm
libjpeg-turbo-1.2.1-3.el6_5.x86_64.rpm
libmono-2_0-1-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
libmono-2_0-devel-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
libmonoboehm-2_0-1-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
libmonoboehm-2_0-devel-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
libmonosgen-2_0-1-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
libmonosgen-2_0-devel-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
libpng-1.2.49-2.el6_7.x86_64.rpm
libSM-1.2.1-2.el6.x86_64.rpm
libtiff-3.9.4-21.el6_8.x86_64.rpm
libX11-1.6.4-3.el6.x86_64.rpm
libX11-common-1.6.4-3.el6.noarch.rpm
libXau-1.0.6-4.el6.x86_64.rpm
libxcb-1.12-4.el6.x86_64.rpm
libXrender-0.9.10-1.el6.x86_64.rpm
mono-complete-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-core-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-data-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-data-oracle-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-data-sqlite-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-devel-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-extras-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-locale-extras-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-mvc-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-nunit-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-reactive-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-wcf-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-web-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-winforms-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
mono-winfxcore-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
monodoc-core-5.12.0.233-0.xamarin.3.epel6.x86_64.rpm
perl-5.10.1-144.el6.x86_64.rpm
perl-libs-5.10.1-144.el6.x86_64.rpm
perl-Module-Pluggable-3.90-144.el6.x86_64.rpm
perl-Pod-Escapes-1.04-144.el6.x86_64.rpm
perl-Pod-Simple-3.13-144.el6.x86_64.rpm
perl-version-0.77-144.el6.x86_64.rpm
pixman-0.32.8-1.el6.x86_64.rpm
referenceassemblies-pcl-2014.04.14-0.xamarin.1.epel6.noarch.rpm
unzip-6.0-5.el6.x86_64.rpm
xsp-4.5-0.xamarin.1.epel6.x86_64.rpm
xamarin.gpg

#In order to create a local repo that is a replica of the online mono repo we have to install mono on the machine used to download online packages
yum install -y mono-complete xsp referencesassemblies-pcl

#following command will get a list of packages related to mono and format to be in a single line separated by space
#this will be used to create 'repodata' which is required when creating a local yum repo
yum list installed | grep mono-centos6-stable | awk '{print $1}' | tr '\n' ' ' >> /apps/mono/packages.txt

#note: we are sending output to file to ensure we don't miss any package name 

ibm-data-db2.x86_64 libgdiplus-devel.x86_64 libgdiplus0.x86_64 libmono-2_0-1.x86_64 libmono-2_0-devel.x86_64 libmonoboehm-2_0-1.x86_64 libmonoboehm-2_0-devel.x86_64 libmonosgen-2_0-1.x86_64 libmonosgen-2_0-devel.x86_64 mono-complete.x86_64 mono-core.x86_64 mono-data.x86_64 mono-data-oracle.x86_64 mono-data-sqlite.x86_64 mono-devel.x86_64 mono-extras.x86_64 mono-locale-extras.x86_64 mono-mvc.x86_64 mono-nunit.x86_64 mono-reactive.x86_64 mono-wcf.x86_64 mono-web.x86_64 mono-winforms.x86_64 mono-winfxcore.x86_64 monodoc-core.x86_64 xsp.x86_64 

#using the output of the previous script run the following command to create yum group for mono
#you have to install yum-utils and createrepo 
yum install -y yum-utils createrepo

#create package group - mono-5.12
cd /apps/mono/packages/mono-5.12

yum-groups-manager --name mono-5.12 --id=mono-5.12 --description 'Group of packages required to install mono 5.12' --save mono-5.12.xml ibm-data-db2.x86_64 libgdiplus-devel.x86_64 libgdiplus0.x86_64 libmono-2_0-1.x86_64 libmono-2_0-devel.x86_64 libmonoboehm-2_0-1.x86_64 libmonoboehm-2_0-devel.x86_64 libmonosgen-2_0-1.x86_64 libmonosgen-2_0-devel.x86_64 mono-complete.x86_64 mono-core.x86_64 mono-data.x86_64 mono-data-oracle.x86_64 mono-data-sqlite.x86_64 mono-devel.x86_64 mono-extras.x86_64 mono-locale-extras.x86_64 mono-mvc.x86_64 mono-nunit.x86_64 mono-reactive.x86_64 mono-wcf.x86_64 mono-web.x86_64 mono-winforms.x86_64 mono-winfxcore.x86_64 monodoc-core.x86_64 xsp.x86_64

#this will create mono-5.12.xml file in the dir /apps/mono/packages/mono-5.12
#this file serves as metadata for the local repo listing all the package dependencies
#check the contents of the xml
cat mono-5.12.xml

#you should see something like the one below

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE comps PUBLIC "-//Red Hat, Inc.//DTD Comps info//EN" "comps.dtd">
<comps>

  <group>
   <id>mono-5.12</id>
   <default>false</default>
   <uservisible>true</uservisible>
   <display_order>1024</display_order>
   <name>mono-5.12</name>
   <description>Group of packages required to install mono 5.12</description>
    <packagelist>
      <packagereq type="default">ibm-data-db2</packagereq>
      <packagereq type="default">libgdiplus-devel</packagereq>
      <packagereq type="default">libgdiplus0</packagereq>
      <packagereq type="default">libmono-2_0-1</packagereq>
      <packagereq type="default">libmono-2_0-devel</packagereq>
      <packagereq type="default">libmonoboehm-2_0-1</packagereq>
      <packagereq type="default">libmonoboehm-2_0-devel</packagereq>
      <packagereq type="default">libmonosgen-2_0-1</packagereq>
      <packagereq type="default">libmonosgen-2_0-devel</packagereq>
      <packagereq type="default">mono-complete</packagereq>
      <packagereq type="default">mono-core</packagereq>
      <packagereq type="default">mono-data</packagereq>
      <packagereq type="default">mono-data-oracle</packagereq>
      <packagereq type="default">mono-data-sqlite</packagereq>
      <packagereq type="default">mono-devel</packagereq>
      <packagereq type="default">mono-extras</packagereq>
      <packagereq type="default">mono-locale-extras</packagereq>
      <packagereq type="default">mono-mvc</packagereq>
      <packagereq type="default">mono-nunit</packagereq>
      <packagereq type="default">mono-reactive</packagereq>
      <packagereq type="default">mono-wcf</packagereq>
      <packagereq type="default">mono-web</packagereq>
      <packagereq type="default">mono-winforms</packagereq>
      <packagereq type="default">mono-winfxcore</packagereq>
      <packagereq type="default">monodoc-core</packagereq>
      <packagereq type="default">xsp</packagereq>
    </packagelist>
  </group>
</comps>

#we will need yum-utils and createrepo to create a local yum repository
yum install -y yum-utils createrepo

#create repo
createrepo -g /apps/mono/packages/mono-5.12/mono-5.12.xml /apps/mono/packages/mono-5.12

#this will create dir /apps/mono/packages/mono-5.12/repodata
#we are all set to proceed to Stage2 i.e. offline install using a local yum repo

#[optional] xamarin.gpg which is a hash file for the package version from mono site: https://download.mono-project.com/repo/xamarin.gpg
#it is only required if you wish to perform a gpg check during install 
#we are not using it for this installation


#Stage2 - Offline install from local yum repo
#this is stage2 and installs mono in offline mode on the target VM using the downloaded rpm packages '/apps/mono/packages/mono-5.12' created in Stage1

#copy packages using ftp or winscp to the preferred directory on the target VM
#in this case it will be /apps/mono/packages/mono-5.12

#update repo file locally and then copy to etc/yum.repos.d/ folder
#give repo file a suitable name e.g. mono-5.12-offline.repo 
#note in this instance the latest version of mono is 5.12
#you can use any text editor, in this instance we are using vim
vi /apps/mono/packages/mono-5.12/mono-5.12-offline.repo

[mono-5.12-offline]
name=mono-5.12-offline
baseurl=file:///apps/mono/packages/mono-5.12/
enabled=1
gpgcheck=0
gpgkey=file:///apps/mono/packages/mono-5.12/xamarin.gpg

#above mono-5.12-offline.repo file is saying that for mono-5.12-offline repository look for rpm packages in the dir /apps/mono/packages/mono-5.12
#note gpgcheck is disabled which means xamarin.gpg file will not be used so leave it set to 0


#copy mono-5.12-offline.repo file to yum.repos.d directory to register it with yum
cp /apps/mono/packages/mono-5.12/mono-5.12-offline.repo /etc/yum.repos.d

#we are now all set to install mono in offline mode
#do a quick check of yum repositories
yum repolist

#look for mono-5.12-offline in the list

#Install mono!
yum install ibm-data-db2.x86_64 libgdiplus-devel.x86_64 libgdiplus0.x86_64 libmono-2_0-1.x86_64 libmono-2_0-devel.x86_64 libmonoboehm-2_0-1.x86_64 libmonoboehm-2_0-devel.x86_64 libmonosgen-2_0-1.x86_64 libmonosgen-2_0-devel.x86_64 mono-complete.x86_64 mono-core.x86_64 mono-data.x86_64 mono-data-oracle.x86_64 mono-data-sqlite.x86_64 mono-devel.x86_64 mono-extras.x86_64 mono-locale-extras.x86_64 mono-mvc.x86_64 mono-nunit.x86_64 mono-reactive.x86_64 mono-wcf.x86_64 mono-web.x86_64 mono-winforms.x86_64 mono-winfxcore.x86_64 monodoc-core.x86_64 xsp.x86_64



#uninstall package
#get list of packages to be uninstalled
yum list installed | grep mono | awk '{print $1}' | tr '\n' ' ' 

#run yum remove using the output of previous script
yum remove ibm-data-db2.x86_64 libgdiplus-devel.x86_64 libgdiplus0.x86_64 libmono-2_0-1.x86_64 libmono-2_0-devel.x86_64 libmonoboehm-2_0-1.x86_64 libmonoboehm-2_0-devel.x86_64 libmonosgen-2_0-1.x86_64 libmonosgen-2_0-devel.x86_64 mono-complete.x86_64 mono-core.x86_64 mono-data.x86_64 mono-data-oracle.x86_64 mono-data-sqlite.x86_64 mono-devel.x86_64 mono-extras.x86_64 mono-locale-extras.x86_64 mono-mvc.x86_64 mono-nunit.x86_64 mono-reactive.x86_64 mono-wcf.x86_64 mono-web.x86_64 mono-winforms.x86_64 mono-winfxcore.x86_64 monodoc-core.x86_64 xsp.x86_64
