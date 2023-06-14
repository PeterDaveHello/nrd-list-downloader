# nrd-list-downloader

nrd-list-downloader is a shell script that automatically downloads, decompresses, and aggregates Newly Registered Domain (NRD) lists from [WhoisDS.com](https://www.whoisds.com/newly-registered-domains). The aggregated NRD lists can be easily used with popular domain blocking tools like [Pi-Hole][Pi-Hole], [AdGuard Home](https://github.com/AdguardTeam/AdGuardHome), and [Blocky][Blocky] to enhance your network security.

## What are Newly Registered Domains (NRDs)?

Newly Registered Domains (NRDs) are domain names that have been registered very recently. They often pose risks as they may not yet be operational and are frequently used for malicious purposes. For instance, NRDs can be [parked domains][Domain Parking Wikipedia] that contain advertisements or are held for sale, provided by default by domain registrars or resellers. Hackers commonly utilize NRDs to spread malware, host phishing sites, and engage in other malicious activities. Understanding the potential threats posed by NRDs is crucial for strengthening cybersecurity measures.

## Why we need NRD list?

NRD lists can be utilized with DNS blocking tools such as Pi-Hole, AdGuard Home, or Blocky. By integrating NRD lists, these tools can reinforce your cybersecurity measures in conjunction with other [threat-hostlist][threat-hostlist].

NRDs are sometimes risky, as hackers like to use them to spread malware, phishing sites, cybercrime, or engage in other malicious activities. Many NRDs used for harmful purposes are short-lived, making traditional security intelligence-based protection less effective against these threats. Though there are also legitimate websites using NRDs, blocking all NRDs might cause false positives. However, the blocking of benign NRDs will only be a short while. Considering the pros and cons, it's worthy to do so.

Some well-known DNS providers like Cisco Umbrella, Akamai ETP(Enterprise Threat Protector), and NextDNS also provide the option to block NRD on their services.

## Dependencies

This script uses several common command-line tools. Ensure you have the following installed:

- `mkdir`: for creating directories
- `wc`: for counting lines and words
- `base64`: for encoding and decoding Base64 data
- `curl`: for downloading content from remote URLs
- `cat`: for concatenating and displaying file contents
- `zcat`: for reading gzip compressed data
- `mktemp`: for creating temporary files
- `date`: for displaying date and time in the system
- `tr`: for character translation, such as deleting characters
- `realpath`: for normalizing file path names
- `dirname`: for extracting the directory name of a file

If any of these tools are missing, you can usually install them using your system's package manager (e.g., `apt`, `yum`, or `pacman`).

## Usage

To get started, clone this repository or download the `nrd-list-downloader.sh` script and grant it execute permission. Then, run the script in the terminal:

```sh
~/nrd-list-downloader $ ./nrd-list-downloader.sh
```

By default, the script downloads and aggregates free NRD data from the past 7 days, saving each day's data in `daily/free/` and combining it into a single file `nrd-7days-free.txt`:

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
...
NRD list for the last 7 days saved to nrd-7days-free.txt, 745232 domains found.
```

To specify a custom duration of data, set the `$DAY_RANGE` variable, such as `14`:

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

The result will be saved in `nrd-14days-free.txt`. The script will not re-download existing daily data in the `daily/free/` directory, saving time and avoiding redundant downloads.

We recommend running nrd-list-downloader regularly (daily or weekly) to keep your NRD lists updated and maximize the effectiveness of your domain blocking tools.

### Paid Account Support

This script also supports paid accounts on WhoisDS.com for accessing premium data. To use a paid account, set the following two variables:

- `PAID_WHOISDS_USERNAME`
- `PAID_WHOISDS_PASSWORD`

You can set the username and password by directly modifying the script or by passing them as environment variables when running the script:

```sh
~/nrd-list-downloader $ PAID_WHOISDS_USERNAME="johndoe@unknownexample.net" PAID_WHOISDS_PASSWORD="RAND.mPA$$W0rd" ./nrd-list-downloader.sh
```

Paid data will be stored in the `daily/paid/` directory and aggregated into the `nrd-7days-paid.txt` file.

The script does not include a mechanism for checking the validity of these credentials, so ensure the information provided is accurate.

## NRD Risk Reference

Here are some references that show you why NRDs are risky. You can take a look, and decide to block them or not.

- Newly Registered Domains: Malicious Abuse by Bad Actors - Unit 42 - Palo Alto Networks
  - <https://unit42.paloaltonetworks.com/newly-registered-domains-malicious-abuse-by-bad-actors/>
- Block newly-registered domains to reduce security threats - The State of Security
  - <https://www.tripwire.com/state-of-security/featured/block-newly-registered-domains-to-reduce-security-threats-in-your-organisation/>
- Should We Blocklist Newly Registered Domains? - Uptycs
  - <https://www.uptycs.com/blog/should-we-blocklist-newly-registered-domains>
- Why blocking NRDs ? - The Newly Registered Domains category | Artica Wiki
  - <https://wiki.articatech.com/filtering-service/newly-registered-domains>

Explore these articles and others to better understand the risks associated with NRDs and determine the most appropriate approach for your organization.

## Credits

Special thanks to WhoisDS.com, although they do not require any mentions in their license. They kindly provide those data for free, even for commercial use, without restrictions.

Quoted from <https://www.whoisds.com/newly-registered-domains>:

> The data provided below is daily list of Newly Registered Domains without whois database downloaded free of charge; except where otherwise stated, they may be reused, including for commercial purposes, without a license and without any payment.

## License

GPL-3.0 (GNU GENERAL PUBLIC LICENSE Version 3)

[Pi-Hole]: https://pi-hole.net/
[AdGuard Home]: https://pi-hole.net/
[Blocky]: https://0xerr0r.github.io/blocky
[threat-hostlist]: https://github.com/PeterDaveHello/threat-hostlist
[Domain Parking Wikipedia]: (https://en.wikipedia.org/wiki/Domain_parking)
