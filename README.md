# Squid 7.7-1 repository for Ubuntu 26.04 LTS

> Squid is a caching proxy for the Web supporting HTTP, HTTPS, FTP, and more. It reduces bandwidth and improves response times by caching and reusing frequently-requested web pages. Squid has extensive access controls and makes a great server accelerator. It runs on most available operating systems, including Windows and is licensed under the GNU GPL.
> <cite> <http://www.squid-cache.org>

This project provides online repository for Squid 7.7-1 recompiled for Ubuntu 26.04 as explained in the https://github.com/diladele/squid-ubuntu repository. 

This version of Squid is used in Web Safety 9.9. Web Safety for Squid Proxy is an ICAP web filtering server/secure web gateway that integrates with Squid proxy server and provides rich content and web filtering functionality to sanitize Internet traffic passing into an internal home/enterprise network. It may be used to block illegal or potentially malicious file downloads, remove annoying advertisements, check downloaded files for viruses, prevent access to various categories of web sites and block resources with adult/explicit content.

Web Safety also has a user friendly Admin UI that you can use to manage your Squid proxy from the browser. To try it out, have a look at [Virtual Appliance ESXi/Hyper-v](https://www.diladele.com/websafety/download.html), [deploy in Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/diladele.websafety?tab=Overview) or [deploy in Amazon AWS](https://aws.amazon.com/marketplace/pp/B07KJHLHKC). 

## How to Install

If you are installing Squid 7.7 for the first time from this repository, run the following commands as `root` (or use the `install.sh` file in this repo).

```bash
#!/bin/bash

# all packages are installed as root
if [[ $EUID -ne 0 ]]; then
   echo "This script must be run as root" 1>&2
   exit 1
fi

# get diladele apt key, dearmor it and add to trusted storage
curl https://www.diladele.com/pkg/diladele_pub.asc | gpg --dearmor >/etc/apt/trusted.gpg.d/diladele_pub.asc.gpg

# add new repo
echo "deb https://diladele.github.io/repo-squid-7_7_1-ubuntu-26_04/repo/ubuntu/ resolute main" \
   >/etc/apt/sources.list.d/squid-7_7_1.diladele.github.io.list

# and install
apt update && apt install -y squid-openssl

# create the override folder for squid
mkdir -p /etc/systemd/system/squid.service.d/

# and override the default number of file descriptors
cat >/etc/systemd/system/squid.service.d/override.conf << EOL
[Service]
LimitNOFILE=65535
EOL

# finally reload the systemd
systemctl daemon-reload
```

If you have installed previous versions of Squid 7 from this repo then simply run `sudo apt update && sudo apt upgrade`. Also check that your current `squid.conf` file from previous version is not overwritten.

## Help

All questions/comments and suggestions are welcome at support@diladele.com or in squid mailing list http://www.squid-cache.org/Support/mailing-lists.html. Squid documentation can be found at http://www.squid-cache.org

## Credits

We admire people working on Squid Cache server, who spend their time free of charge and deliver great product to all of us.
