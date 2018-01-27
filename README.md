# guacamole
Guacamole is a program to control a Linux desktop over the network in a browser. Sometimes in your Linux life, you need to control your servers in the internet with a graphical user interface. ... One way to go is to use a browser to display a Linux desktop. The solution is guacamole
Pacakages for ubuntu 16.04 :-

# sudo apt-get install make libssh2-1-dev libtelnet-dev libpango1.0-dev libossp-uuid-dev libcairo2-dev libpng12-dev freerdp-x11 libssh2-1 libvncserver-dev libfreerdp-dev libvorbis-dev libssl0.9.8 gcc libssh-dev libpulse-dev tomcat7 tomcat7-admin tomcat7-docs ghostscript

Now test tomcat works properly or not with below syntex:- 
http://servername-ip:8080
Output :- IT works!
Release Version:-
Guacamole 0.9.6
# cd /usr/src

# wget http://downloads.sourceforge.net/project/guacamole/current/source/guacamole-server-0.9.6.tar.gz
# wget http://downloads.sourceforge.net/project/guacamole/current/binary/guacamole-0.9.6.war

# tar xvzf guacamole-server-0.9.6.tar.gz
# cd /usr/src/guacamole-server-0.9.6
# ./configure --with-init-dir=/etc/init.d
It will throw below output :-  
------------------------------------------------
guacamole-server version 0.9.6
------------------------------------------------

   Library status:

     freerdp ............. yes
     pango ............... yes
     libssh2 ............. yes
     libssl .............. yes
     libtelnet ........... yes
     libVNCServer ........ yes
     libvorbis ........... yes
     libpulse ............ yes

   Protocol support:

      RDP ....... yes
      SSH ....... yes
      Telnet .... yes
      VNC ....... yes

   Init scripts: /etc/init.d

Type "make" to compile guacamole-server.

# make
# make install

TEST SERVER :-
# /etc/init.d/guacd start

Starting guacd: /usr/local/sbin/guacd: error while loading shared libraries: libguac.so.7: cannot open shared object file: No such file or directory
FAIL

So to resolve above error:- run following command :-

# ldconfig
# /etc/init.d/guacd start
Starting guacd: guacd[25039]: INFO:     Guacamole proxy daemon (guacd) version 0.9.5 started
SUCCESS

# /etc/init.d/guacd stop

#############################################################################################

Guacamole server configuration:-

# mkdir /etc/guacamole
# nano /etc/guacamole/guacamole.properties
#############################################################
# -------------------------------------------------------------------- 

# Hostname and port
guacd-hostname: localhost
guacd-port: 4822

# .jar 
lib-directory: /var/lib/tomcat7/webapps/guacamole/WEB-INF/classes

# Welcome page
auth-provider: net.sourceforge.guacamole.net.basic.BasicFileAuthenticationProvider

# BasicFileAuthenticationProvider 
basic-user-mapping: /etc/guacamole/user-mapping.xml

##################################################################################################

# nano /etc/guacamole/user-mapping.xml

<user-mapping>

    <!--  Administrator-->
    <authorize username="admin" password="adminpassword">

        <!-- Verbindung 1 für Benuter admin -->
        <!-- RDP - Remotedesktop-Verbindung -->
        <!-- Parameter siehe http://guac-dev.org/doc/gug/configuring-guacamole.html#rdp -->
        <connection name="Windows 7 Test VM">
                <protocol>rdp</protocol>
                <param name="hostname">192.168.42.40</param>      <!-- FQDN oder IP des Zielhost -->
                <param name="port">3389</param>                   <!-- Port, Standard ist 3389 -->
                <param name="username">Testbenutzer</param>       <!-- Anmeldename / Benutzername -->
                <param name="password">password123</param>        <!-- Password für den Benutzer -->
                <param name="domain">TEST-VM</param>              <!-- Domäne des Benutzer, ggf. Hostname des Ziels -->
                <param name="disable-audio">true</param>          <!-- Audio-Übertragung deaktivieren -->
                <param name="console">true</param>                <!-- sorgt z.B. bei Terminalserver dafür die Consolen-Sitzung zu bekommen, ansonsten sinnlos -->
                <param name="server-layout">de-de-qwertz</param>  <!-- mit deutscher Tastatur verbinden -->
                <param name="ignore-cert">true</param>            <!-- alle Zertifikate akzeptieren -->
        </connection>

        <!-- Verbindung 2 für Benuter admin -->
        <!-- SSH - Verbindung -->
        <!-- Parameter siehe http://guac-dev.org/doc/gug/configuring-guacamole.html#ssh -->
        <connection name="SSH Webserver">
                <protocol>ssh</protocol>
                <param name="hostname">192.168.42.10</param>      <!-- FQDN oder IP des Zielhost -->
                <param name="port">22</param>                     <!-- Port, Standard ist 22 -->
                <param name="username">user23</param>             <!-- Anmeldename / Benutzername -->
                <param name="password">password123</param>        <!-- Password für den Benutzer -->
        </connection>
    </authorize>
</user-mapping>

######################################################################################################################

# update-rc.d guacd defaults

# ln -s /usr/local/lib/freerdp/guacdr.so /usr/lib/x86_64-linux-gnu/freerdp/
# ln -s /usr/local/lib/freerdp/guacsnd.so /usr/lib/x86_64-linux-gnu/freerdp/

# service guacd start
http://FQDN-oder-IP:8080/guacamole/

