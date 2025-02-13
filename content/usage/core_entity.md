+++
title = "Core Entity Descriptions"
description = "Details about the Core Entities"
weight = 4
+++

Core entities use regular expressions to find Entities with in HTML and Plain text Entries and Alerts.  Listed below are the current set of core entities and some notes about each.

## CVE 

This detection finds references to Common Vulnerability and Exposures IDs.  See [CVE.or](https://cve.org) for more information.  This core entity looks for a string in format of CVE-YYYY-DDDD is discovered.  YYYY is the a four digit year and DDDD represents 4 or more digits after the last hyphen.

## Browser User Agent String

Due the vast array of potential (user agent strings)[https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent], this core entity is experimental, and will only reliably detect more common user agent strings.

## CIDR

SCOT's flair engine will detect CIDR blocks ((CIDR)[https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing]).  A CIDR block is essentially an IP address with a slash and a two digit number at the end.

## IPv4 Addresses

Flair will detect (IP version 4)[https://en.wikipedia.org/wiki/IPv4] string addresses commonly displayed as w.x.y.z, where w, x, y, and z are all numbers between 0 and 255.  Flair will (Deobfuscate)[#Deobfuscate](#Deobfuscation)

## IPv6 Addresses

Flair will detect IPv6 Addresses. The following examples all will be detected:

* 1762::a03:1:af18
* 1763:0:0:0:0:B03:1:AF18
* 2001:41c0:2:9d17::
* 2001:489b:2202:2000:0000:0000:0000:0009:53

Vendors are constantly inventing new ways to mis-display IPv6, so if you find an address that is not flairing, please file a bug report.

## Domain Names

Domain names that have top level domains listed in Mozilla's (Public Suffix List)[https://publicsuffix.org/] will be matched.  

Another thing that makes domain names fun is the potential for them to be written in (punycode)[https://en.wikipedia.org/wiki/Punycode].  The Flair engine will handle most punycode.

## Hashes

Hashes, you love 'em, we have 'em.  Flair will detect MD5, SHA1 and SHA256 hashes.

## Email Addresses

Email addresses are two for one special!  Not only will Flair detect the e-mail address, but you will get a bonus domain name entity as well.  For example scot@foo.com will yield two entities: scot@foo.com (type=email) and foo.com (type=domain).

## Filenames with common extensions

Filenames can be anything these days.  Gone are the good old days when filenames always ended in a 3 letter extension.  Well, Flair is nostalgic, and can only reliably detect filenames that end in common 3 letter extension.  See (Common File Extensions)[#common-file-extensions] below.

## UUIDs

Flair detects these (Universal Unique Identifiers)[https://en.wikipedia.org/wiki/Universally_unique_identifier].  

## Clsids

Flair also detects (CSLID)[https://learn.microsoft.com/en-us/windows/win32/com/clsid-key-hklm] keys.

## Email Message-Ids

What looks like an email address, but isn't? Why an (email Message-Id)[https://en.wikipedia.org/wiki/Message-ID]!  Flair gets this right, some of the time.  

## WinRegistry Keys

Put a (WinRegistry Key)[https://en.wikipedia.org/wiki/Windows_Registry#:~:text=The%20registry%20contains%20two%20basic,its%20contained%20subkeys%20and%20values.] into your data, and Flair will find it.

## Jarm Hash

(Jarm Hashes)[https://securitytrails.com/blog/jarm-fingerprinting-tool] will be identified by Flair.

## Country names

Common American English spellings of various countries will be detected and returned as Entities.

## Internal Scot Links

Flair can detect two types of internal links.  The first is a special string pattern of the form: Scot-Thing-Number, where Thing is something like "Alertgroup", "Intel", "Event", etc. and Number is a integer that refers to the id number of the thing.  The other is a URL that points to your scot server like: https://scot.watermelon.com/#/Dispatch/13543.  

In either case, Flair will replace the internal link with something like: &lt;a href="https://scot.watermelon.com/#/Dispatch/13543"&gt;Scot-Dispatch-13543&lt;/a&gt;.  

In neither case will Flair create an entity for this link.  

Oh yeah, the scot-thing-number is case insensitive.

## SIDs

(Security Identifier)[https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-identifiers] will be detected and flair-ed.

# Notes

## Deobfuscation

It used to be common practice to obfuscation IP addresses and domain names when including them in reporting.  The idea was that by slightly changing the address or domain name the human reader would be able to recognize the address but clicking on it in a browser would not inadvertently send the user to a potentially dangerous location.

Common means to obfuscate an IP address or domain name where to wrap the periods that separate the parts of the address with some kind of bracket.  Most commonly, parenthesis, curly brackets, or square brackets were used.  Depending on the author, either all periods would be wrapped or only some of them would be.  For example:

* 172(.)16(.)1(.).1
* 172{.}16{.}1{.}.1
* 172[.]16[.]1[.].1
* 172(.)16{.}1[.].1 
* 172.16.1(.)1
* foo.domain[.]com

would all be commonly seen.

SCOT's first "customer" considered obfuscation as visual noise that distracted from seeing and recognizing the address.  Also, the wide variability of styles, made it difficult to match 172(.)16.1.1 to 172.16(.)1.1 and have it refer to a singular IP address.  Therefore, SCOT's flair engine will match any of the above styles of obfuscation, but will remove it (deobfuscate) for display and creation of Entity records.

## common file extensions

To detect files from random strings, Flair looks for these common file extensions:

```
    7z|arg|deb|pkg|rar|rpm|tar|tgz|gz|z|zip|                  # compressed
    aif|mid|midi|mp3|ogg|wav|wma|                             # audio
    bin|dmg|iso|exe|bat|                                      # executables
    csv|dat|log|mdb|sql|xml|                                  # db/data
    eml|ost|oft|pst|vcf|                                      # email
    apk|bat|bin|cgi|exe|jar|                                  # executable
    fnt|fon|otf|ttf|                                          # fonts
    ai|bmp|gif|ico|jpeg|jpg|ps|png|psd|svg|tif|tiff|          # images
    asp|aspx|cer|cfm|css|htm|html|js|jsp|part|php|rss|xhtml|  # web serving
    key|odp|pps|ppt|pptx|                                     # presentation
    c|class|cpp|h|vb|swift|py|rb|                             # source code
    ods|xls|xlsm|xlsx|                                        #spreadsheats
    cab|cfg|cpl|dll|ini|lnk|msi|sys|                          # misc sys files
    3g2|3gp|avi|flv|h264|m4v|mkv|mov|mp4|mpg|mpeg|vob|wmv|    # video
    doc|docx|odt|pdf|rtf|tex|txt|wpd|                         # word processing
    jse|jar|
    ipt|
    hta|
    mht|
    ps1|
    sct|
    scr|
    vbe|vbs|
    wsf|wsh|wsc
```

Feel free to file a feature request if you would like to see different ones.
