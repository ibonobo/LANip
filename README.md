# LANip
scripts and techniques to relay the LANip for a headless server  

I have a laptop hybrid with a keyboard problem. Because of its tablet capabilities, not having a keyboard is not the end of the world, and I had installed Kali (based on Debian) as the second OS. I sometimes travel with it and want to have the ability to SSH into it, but for that I need the LAN IP. Router access is not always on the menu, so I cannot always find it that way.  

I looked around and I've found that a lot of effort has gone into creating scripts that email the WAN IP when it changes. That is not helpful for me. Besides, if such changes were of interest to me I would simply install a DynDNS client (or OpenDNS for multi) - I find HE.net and afraid.org to be best suited for this purpose. Still, for the sake of completion, I will include some of the scripts I found doing this.  
1. aneisch/Report-IP bash script sends IP from both eth0 and wlan0 on boot. Requires mailx. sudo apt-get install heirloom-mailx . Add to crontab with @reboot /path/to/script or use rc.local to run at boot
2. graysky2/checkip seems rather complicated, needing make and depending on dig, curl, s-nail and / or mailsend
3. Morrolan/check_ip_no_auth.py is a Python script that requires Google Mail credentials, among other things  
4. txt3rob/SMS-PI/tree/master/Examples/PHP/SMS%20IP PHP script to text (SMS) the IP via a UK service
5. http://elinux.org/RPi_Email_IP_On_Boot_Debian - Python script for Raspberry Pi owners, msg['Subject'] = 'IP For %s on %s' % (socket.gethostname(), today.strftime('%b %d %Y'))
6. http://www.techrepublic.com/blog/linux-and-open-source/support-trick-automatically-receive-the-ip-addresses-of-remote-computers/ TechRepublic relies on Mutt and Bash

I prefer a solution that simply displays the IP on the screen before login (and hopefully with the MAC as well, since I have macchanger changing it on every boot). The following use /etc/issue
1. http://superuser.com/questions/380550/modify-login-prompt-or-header-etc-issue-to-display-ip-address-of-the-machine - to put it in /etc/issue : ifconfig eth0 | awk '/inet addr/ {print $2}' | cut -f2 -d: > /etc/issue
2. http://offbytwo.com/2008/05/09/show-ip-address-of-vm-as-console-pre-login-message.html
3. http://serverfault.com/questions/209599/how-to-setup-etc-issues-to-show-the-ip-address-for-eth0
4. http://stackoverflow.com/questions/3769254/linux-display-ipaddres-before-logon
5. http://raspberrypi.stackexchange.com/questions/1409/easiest-way-to-show-my-ip-address
6. http://askubuntu.com/questions/217358/how-can-i-display-eth0s-ip-address-at-the-login-screen-on-precise-server

There are basically three ways to get the IP via CLI:
'/sbin/ifconfig -a | grep inet'
'hostname -I'
'ip addr show' or simply 'ip a'
The 3rd is probably best but for some reason the first is the most popular.

Sometimes I can nmap the network, for instance >nmap -O 192.168.1.0/24 --exclude 192.168.1.1,192.168.1.5 (an excluded list could be created with --excludefile, -p ** for port) see also http://www.edugeek.net/forums/general-chat/100535-get-ip-address-into-text-file.html
ipconfig /all | Find "IPv4">c:\ipaddy.txt
