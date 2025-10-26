+++
title = 'ddclient'
date = 2025-10-20T19:32:56+01:00
draft = false
[cascade]
	type = 'blog'
+++

It is important to keep your domain(s) pointing at the right IP address, some
ISPs provide dynamic ip address to customers which means occasionally updating
your services to keep them working. Cloudflare recomends ddclient to update the
DNS record(s), it also supports many other providers so is a good place to start.


{{% details title="Dynamic DNS services supported" closed="true"%}}
List correct at time of writing for ddclient v4.0.0
* [1984.is](https://www.1984.is/product/freedns)
* [ChangeIP](https://www.changeip.com)
* [Cloudflare](https://www.cloudflare.com)
* [ClouDNS](https://www.cloudns.net)
* [DDNS.fm](https://www.ddns.fm/)
* [DigitalOcean](https://www.digitalocean.com/)
* [dinahosting](https://dinahosting.com)
* [Directnic](https://directnic.com)
* [DonDominio](https://www.dondominio.com)
* [DNS Made Easy](https://dnsmadeeasy.com)
* [DNSExit](https://dnsexit.com/dns/dns-api)
* [dnsHome.de](https://www.dnshome.de)
* [Domeneshop](https://api.domeneshop.no/docs/#tag/ddns/paths/~1dyndns~1update/get)
* [DslReports](https://www.dslreports.com)
* [Duck DNS](https://duckdns.org)
* [DynDNS.com](https://account.dyn.com)
* [EasyDNS](https://www.easydns.com )
* [Enom](https://www.enom.com)
* [Freedns](https://freedns.afraid.org)
* [Freemyip](https://freemyip.com)
* [Gandi](https://gandi.net)
* [GoDaddy](https://www.godaddy.com)
* [Hurricane Electric](https://dns.he.net)
* [Infomaniak](https://faq.infomaniak.com/2376)
* [INWX](https://www.inwx.com/)
* [Loopia](https://www.loopia.se)
* [Mythic Beasts](https://www.mythic-beasts.com/support/api/dnsv2/dynamic-dns)
* [NameCheap](https://www.namecheap.com)
* [NearlyFreeSpeech.net](https://www.nearlyfreespeech.net/services/dns)
* [Njalla](https://njal.la/docs/ddns)
* [Noip](https://www.noip.com)
* nsupdate - see nsupdate(1) and ddns-confgen(8)
* [OVH](https://www.ovhcloud.com)
* [Porkbun](https://porkbun.com)
* [regfish.de](https://www.regfish.de/domains/dyndns)
* [Sitelutions](https://www.sitelutions.com)
* [Yandex](https://dns.yandex.com)
* [Zoneedit](https://www.zoneedit.com)
{{% /details %}}

## Setup

Get an [API Token](https://dash.cloudflare.com/profile/api-tokens) from Cloudflare
* Click "Create Token"
* Use the "Edit zone DNS" template
* Set two permissions with "Zone" "DNS" "Read" and "Zone" "DNS" "Edit"
* Set Zone Resources to "Include" "All zones from an account" "&lt;YOUR ACCOUNT&gt;"
* Client IP Address Filtering and TTL can be left blank
* Continue to summary
* Create Token
* Copy the token into a notepad and test that it works from the system that will be
updating the DNS using the provided script


## Installation

Most package managers have a version that will install under the name `ddclient`
however it many versions are not up to date and some come under a slightly
different name. It is therefore best to install by cloning the repo and running
the commands listed to make and install the package yourself.

{{< cards >}}
  {{< card link="https://github.com/ddclient/ddclient?tab=readme-ov-file#installation" title="ddclient github installation" icon="github" >}}
{{< /cards >}}

More info on step 3 below


## Configuration

Using your favourite editor open `/etc/ddclient/ddclient.conf` It will be full
of templates for all of the supported services, this can all be deleted as we
are focusing on Cloudflare. 

```markdown {filename=ddclient.conf, hl_lines=[24,31]}
## Use encryption (TLS) when the scheme (either "http://" or "https://") is
## missing from a URL.  Defaults to "yes".
ssl=yes

daemon=300                      # check every 300 seconds
syslog=yes                      # log update msgs to syslog
mail=root                       # mail all msgs to root
mail-failure=root               # mail failed update msgs to root
# mail-from=root                # set the email "From:" header to "root".  If
                                # unset (the default) or empty, the from address
                                # depends on your system's default behavior.
pid=/var/run/ddclient.pid  # record PID in file.
# postscript=script             # run script after updating.  The new IP is
                                # added as argument.
use=web, web=api.ipify.org		# Obtain an IP address from Web status page

#
# Cloudflare (www.cloudflare.com)
# 	
protocol=cloudflare,                   \
zone=mostdiv.co.uk,                    \
ttl=1,                                 \
login=token,                           \ # Only needed if you are using your global API key. If you are using an API token, set it to "token" (without double quotes).
password=<API Token>             \ # This is either your global API key, or an API token. If you are using an API token, it must have the permissions "Zone - DNS - Edit" and "Zone - Zone - Read". The Zone resources must be "Include - All zones".
mostdiv.co.uk, www.mostdiv.co.uk <Other subdomains here>

protocol=cloudflare,                   \
zone=mostdiv.uk,                       \
ttl=1,                                 \
login=token,                           \
password=<API Token>             \ # This is either your global API key, or an API token. If you are using an API token, it must have the permissions "Zone - DNS - Edit" and "Zone - Zone - Read". The Zone resources must be "Include - All zones".
mostdiv.uk, www.mostdiv.uk  <Other subdomains here>
```