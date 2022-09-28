# nrd-list-downloader

Shell script to download NRD(Newly Registered Domain) list from [WhoisDS.com](https://www.whoisds.com/newly-registered-domains), and auto decompress, aggregate the domain list into a single file, that can be used with [Pi-Hole][Pi-Hole], [AdGuard Home][AdGuard Home], [Blocky][Blocky], or other domain blocker.

# Usage

Use git to clone this repository, or download the `nrd-list-downloader.sh` script and give it execute permission, then run it under terminal like:

```sh
~/nrd-list-downloader $ ./nrd-list-downloader.sh
```

By default, it will scrape the free NRD data for the last 7 days, save each daily data into `daily/free/`, and aggregated data into `nrd-7days-free.txt`:

```sh
~/nrd-list-downloader $ ./nrd-list-downloader.sh
You are using nrd-list-downloader to download NRD(Newly Registered Domain) list ...
NRD list of the last 7 will be downloaded.

Download and decompress 2022-09-21 data ...137691 domains found.
Download and decompress 2022-09-22 data ...55127 domains found.
Download and decompress 2022-09-23 data ...187397 domains found.
Download and decompress 2022-09-24 data ...118922 domains found.
Download and decompress 2022-09-25 data ...60806 domains found.
Download and decompress 2022-09-26 data ...82211 domains found.
Download and decompress 2022-09-27 data ...103078 domains found.
NRD list for the last 7 days saved to nrd-7days-free.txt, 745232 domains found.
```

You can specify how many days of data do you want to have by using `$DAY_RANGE` variable, for example - `14`:

```sh
~/nrd-list-downloader $ DAY_RANGE=14 ./nrd-list-downloader.sh
You are using nrd-list-downloader to download NRD(Newly Registered Domain) list ...
NRD list of the last 14 will be downloaded.

Download and decompress 2022-09-14 data ...141531 domains found.
Download and decompress 2022-09-15 data ...139059 domains found.
Download and decompress 2022-09-16 data ...129216 domains found.
Download and decompress 2022-09-17 data ...119194 domains found.
Download and decompress 2022-09-18 data ...105884 domains found.
Download and decompress 2022-09-19 data ...86455 domains found.
Download and decompress 2022-09-20 data ...106169 domains found.
daily/free/2022-09-21 existed with 137691 domains, skip the download and decompress process ...
daily/free/2022-09-22 existed with 55127 domains, skip the download and decompress process ...
daily/free/2022-09-23 existed with 187397 domains, skip the download and decompress process ...
daily/free/2022-09-24 existed with 118922 domains, skip the download and decompress process ...
daily/free/2022-09-25 existed with 60806 domains, skip the download and decompress process ...
daily/free/2022-09-26 existed with 82211 domains, skip the download and decompress process ...
daily/free/2022-09-27 existed with 103078 domains, skip the download and decompress process ...
NRD list for the last 14 days saved to nrd-14days-free.txt, 1572740 domains found.
```

The existed daily data in the `daily/free/` directory won't be downloaded again, and the result will be saved to `nrd-14days-free.txt`.

## Paid account support

If you have a paid WhoisDS.com account, this script can also use the account to download paid data.

Two variables are needed to use a paid WhoisDS.com account:

- `PAID_WHOISDS_USERNAME`
- `PAID_WHOISDS_PASSWORD`

Set them in the script, or pass them as environment variable like this:

```sh
~/nrd-list-downloader $ PAID_WHOISDS_USERNAME="johndoe@unknownexample.net" PAID_WHOISDS_PASSWORD="RAND.mPA$$W0rd" ./nrd-list-downloader.sh
```

Paid data will be download into `daily/paid/` directory, and aggregated as `nrd-7days-paid.txt`.

There is no username/password check mechanism in the script, make sure you provide the correct information.

# What are Newly Registered Domains(NRDs)?

Newly registered domains (NRDs) are domains names that have been registered very recently, it's often that a newly registered domain within a few days is not ready in use yet, sometimes, a NRD could also be a [parked domain](https://en.wikipedia.org/wiki/Domain_parking) at the same time, because it's common that a domain registrar or reseller also provide domain parking by default.


# Why we need NRD list?

You can use it as a DNS level blocklist on Pi-Hole, AdGuard Home, or Blocky, along with other [threat-hostlist][threat-hostlist] to block risky domains, to protect your cyber security. Of course, you can use it for research purposes.

NRDs are sometimes risky, hackers like to use a NRD to spread malware, phishing site, cybercrime, or other malicious purposes and attacks.

Many of NRDs that were used for harmful or malicious activities were only alive for a very short while, which caused traditional security intelligence based protection might not be useful enough to deal with those bad domains, though there are also good sites using NRDs, blocking all NRDs could cause false-positives, but the blocking of benign NRDs will only be a short while, by consider the pros and cons, it's worthy to do so.

Some famous DNS providers like Cisco Umbrella and NextDNS also provides the option to block NRD on there service.

## NRD Reference

Here are some references that show you why NRDs are risky. You can take a look, and decide to block them or not.

- Newly Registered Domains: Malicious Abuse by Bad Actors - Unit 42 - Palo Alto Networks
  - https://unit42.paloaltonetworks.com/newly-registered-domains-malicious-abuse-by-bad-actors/
- Block newly-registered domains to reduce security threats - The State of Security
  - https://www.tripwire.com/state-of-security/featured/block-newly-registered-domains-to-reduce-security-threats-in-your-organisation/
- Should We Blocklist Newly Registered Domains? - Uptycs
  - https://www.uptycs.com/blog/should-we-blocklist-newly-registered-domains
- Why blocking NRDs ? - The Newly Registered Domains category | Artica Wiki
  - https://wiki.articatech.com/filtering-service/newly-registered-domains

# Credits

Special thanks to WhoisDS.com, though they don't have a license to require any mentions. They kindly provide those data for free, even for commercial use, without restrictions.

Quote from https://www.whoisds.com/newly-registered-domains

> The data provided below is daily list of Newly Registered Domains without whois database downloaded free of charge; except where otherwise stated, they may be reused, including for commercial purposes, without a license and without any payment.

# License

GPL-3.0 (GNU GENERAL PUBLIC LICENSE Version 3)


[Pi-Hole]: https://pi-hole.net/
[AdGuard Home]: https://pi-hole.net/
[Blocky]: https://0xerr0r.github.io/blocky
[threat-hostlist]: https://github.com/PeterDaveHello/threat-hostlist
