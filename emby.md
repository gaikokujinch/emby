Emby


=======

Source: https://emby.media/freebsd-server.html


ToC

+ [Add user emby](#useremby)
+ [Install](#install)
+ [Configure](#configure)
+ [Run](#run)
+ [Update](#update)



Notes
-----

```
jls
jexec 18 tcsh
```



Inside the jail

Optional: give root a password
```
passwd
```

**Add user emby<a name="#useremby">**

```
adduser
```
Username: emby
Full name: Emby
Uid: (same as in NAS)
Login group: (default)
Login group is emby. Invite emby into other groups: wheel
Login class: (default)
Shell: nologin
Home directory: (default)
Home directory permissions: (default)
Use password-based authentication: no
Lock out the account after creation: (default)
OK?: yes
Add another user?: no




**Install Emby<a name="#install">**


```
pkg update
pkg upgrade
pkg install emby-server
```


**Configure<a name="#configure">**


Remove default FFMpeg package

```
pkg delete -f ffmpeg
```


Reinstall FFMpeg from ports with lame option enabled
```
portsnap fetch extract
portsnap fetch update
cd /usr/ports/multimedia/ffmpeg
make config-recursive
```
enable the lame option
enable the ass subtitles option
enable the opus subtitles option
enable the x265 subtitles option
```
make config-recursive
make install clean
```

**Run<a name="#run">**

```
sysrc emby_server_enable="YES"
service emby-server start
```

Once started, visit the following webpage to configure: http://localhost:8096/


**Update<a name="#update">**

```
jexec 18 tcsh
service emby-server stop
cd /tmp
mkdir emby
cd emby
fetch https://github.com/MediaBrowser/Emby/releases/download/3.2.20.0/Emby.Mono.zip
unzip Emby.Mono.zip
cp -Rfp /tmp/emby/ /usr/local/lib/emby-server/
rm -rf /tmp/emby
service emby-server start
```
To update everything else:
```
jexec 18 tcsh
service emby-server stop
portsnap fetch extract
portsnap fetch update
```
the following command will list the installed ports which are out of date
```
pkg version -l "<"
```
Portmaster is a very small utility for upgrading installed ports. It is designed to use the tools installed with the FreeBSD base system without depending on other ports or databases.
```
cd /usr/ports/ports-mgmt/portmaster
make install clean
```
To list these categories and search for updates:
```
portmaster -L
```
This command is used to upgrade all outdated ports:
```
portmaster -a
```

```
pkg update
pkg upgrade
```
Does this break the ffmpeg?
```
service emby-server start
```