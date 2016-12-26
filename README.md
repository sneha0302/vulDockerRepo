# PHPMailer < 5.2.18 Remote Code Execution
[![Docker Pulls](https://img.shields.io/docker/pulls/vulnerables/exploit-CVE-2016-10033.svg?style=plastic)](https://hub.docker.com/r/vulnerables/exploit-CVE-2016-10033/)
![License](https://img.shields.io/badge/License-GPL-blue.svg?style=plastic)

## Vulnerable environment

To setup a vulnerable environment for your test you will need [Docker](https://docker.com) installed, and just run the following command:

    docker run --rm -it -p 8080:80 vulnerables/cve-2016-10033

## Exploit

## Vulnerable code

Before this [commit](https://github.com/PHPMailer/PHPMailer/commit/4835657cd639fbd09afd33307cef164edf807cdc) in [class.phpmailer.php](https://github.com/opsxcq/exploit-CVE-2016-10033/blob/master/src/class.phpmailer.php#L1446) in a certain scenarion there is no filter in the sender's email address special chars. This flaw can lead to a remote code execution, via ```mail``` function [here](https://github.com/opsxcq/exploit-CVE-2016-10033/blob/master/src/class.phpmailer.php#L700).

To trigger this code, you need:

    * to compile PHP without PCRE.
    * PHP version must be inferior to 5.2.0.

So you can bypass the sender's email validation on ```validateAddress``` function, setting ```patternselect``` to ```noregex```. To make easier to archieve such environment I just changed it to [this code](https://github.com/opsxcq/exploit-CVE-2016-10033/blob/master/src/class.phpmailer.php#L1102).


### Disclaimer

This or previous program is for Educational purpose ONLY. Do not use it without permission. The usual disclaimer applies, especially the fact that me (opsxcq) is not liable for any damages caused by direct or indirect use of the information or functionality provided by these programs. The author or any Internet provider bears NO responsibility for content or misuse of these programs or any derivatives thereof. By using these programs you accept the fact that any damage (dataloss, system crash, system compromise, etc.) caused by the use of these programs is not opsxcq's responsibility.
