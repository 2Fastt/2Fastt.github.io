---
title: Cicada
published: 03 Oct 2024
---


En este writeup, exploramos la máquina Cicada de Hack The Box, donde explotamos Active Directory mediante la recopilación de información y la técnica Pass-the-Hash. Finalmente, logramos comprometer el dominio y extraer datos sensibles.

# [](#header-1)RECONOCIMIENTO

Empezaré por scanear todos los protocolos en la máquina objetivo:
  1. Escanear los puertos abiertos.
  2. Escanear los servicios en cada puerto que esté abierto.
  ```js
  // Javascript code with syntax highlighting.
  nmap -sCV -v 10.10.11.35 -Pn -T3
  ```
  Como resultado encontramos esto:
  ```js
  // Javascript code with syntax highlighting.
  Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
	Starting Nmap 7.93 (https://nmap.org ) at 2024-10-02 12:18 CEST
	NSE: Loaded 155 scripts for scanning.
	NSE: Script Pre-scanning.
	Initiating NSE at 12:18
	Completed NSE at 12:18, 0.00s elapsed
	Initiating NSE at 12:18
	Completed NSE at 12:18, 0.00s elapsed
	Initiating NSE at 12:18
	Completed NSE at 12:18, 0.00s elapsed
	Initiating Parallel DNS resolution of 1 host. at 12:18
	Completed Parallel DNS resolution of 1 host. at 12:18, 0.01s elapsed
	Initiating Connect Scan at 12:18
	Scanning 10.10.11.35 [1000 ports]
	Discovered open port 135/tcp on 10.10.11.35
	Discovered open port 53/tcp on 10.10.11.35
	Discovered open port 445/tcp on 10.10.11.35
	Discovered open port 139/tcp on 10.10.11.35
	Discovered open port 464/tcp on 10.10.11.35
	Discovered open port 389/tcp on 10.10.11.35
	Discovered open port 593/tcp on 10.10.11.35
	Discovered open port 88/tcp on 10.10.11.35
	Discovered open port 3269/tcp on 10.10.11.35
	Discovered open port 3268/tcp on 10.10.11.35
	Discovered open port 636/tcp on 10.10.11.35
	Completed Connect Scan at 12:18, 4.00s elapsed (1000 total ports)
	Initiating Service scan at 12:18
	Scanning 11 services on 10.10.11.35
	Stats: 0:00:12 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
	Service scan Timing: About 63.64% done; ETC: 12:18 (0:00:05 remaining)
	Completed Service scan at 12:19, 45.25s elapsed (11 services on 1 host)
	NSE: Script scanning 10.10.11.35.
	Initiating NSE at 12:19
	Completed NSE at 12:19, 40.04s elapsed
	Initiating NSE at 12:19
	Completed NSE at 12:19, 1.60s elapsed
	Initiating NSE at 12:19
	Completed NSE at 12:19, 0.00s elapsed
	Nmap scan report for 10.10.11.35
	Host is up (0.037s latency).
	Not shown: 989 filtered tcp ports (no-response)
	PORT     STATE SERVICE       VERSION
	53/tcp   open  domain        Simple DNS Plus
	88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-10-02 17:18:36Z)
	135/tcp  open  msrpc         Microsoft Windows RPC
	139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
	389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
	| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
	| Subject Alternative Name: othername:<unsupported>, DNS:CICADA-DC.cicada.htb
	| Issuer: commonName=CICADA-DC-CA
	| Public Key type: rsa
	| Public Key bits: 2048
	| Signature Algorithm: sha256WithRSAEncryption
	| Not valid before: 2024-08-22T20:24:16
	| Not valid after:  2025-08-22T20:24:16
	| MD5:   9ec51a2340efb5b83d2c39d8447ddb65
	|_SHA-1: 2c936d7bcfd811b99f711a5a155d88d34a52157a
	|_ssl-date: TLS randomness does not represent time
	445/tcp  open  microsoft-ds?
	464/tcp  open  kpasswd5?
	593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
	636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
  ```

  En especial encontramos un Active Directory (CICADA-DC.cicada.htb)
## [](#header-2)Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### [](#header-3)Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### [](#header-4)Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### [](#header-5)Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### [](#header-6)Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

### Large image

![](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
