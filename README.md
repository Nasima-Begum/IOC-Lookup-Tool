# IOC Lookup Tool

A small Python tool I built to check IPs, file hashes, domains, URLs, and email headers against AbuseIPDB and VirusTotal — instead of doing it manually one at a time.

## Why I built this

I was working through a home SOC lab (Wazuh + Atomic Red Team + OpenVAS) and kept manually pasting IOCs into different websites to check them. It got old fast, especially when checking more than a couple at once. I looked into how SOAR platforms automate this and decided to build a scaled-down version myself — partly to save time, mostly to actually understand what's happening instead of just clicking buttons in a tool.

It started as a single-IP checker. Once that worked, I added hashes, domains, URLs, and email header analysis one at a time, testing each before moving to the next.

## What it does

- Checks IPs against AbuseIPDB (abuse score, country, ISP, report categories)
- Checks hashes, domains, and URLs against VirusTotal (detection ratio, tags, WHOIS for domains)
- Parses raw email headers — pulls sender IP, From domain, SPF/DKIM/DMARC results, and flags Reply-To mismatches to catch phishing
- Auto-detects what type of IOC you gave it, so you don't have to specify
- Two ways to run it: a terminal version for quick checks, and an Excel version that saves results and color-codes them (red/orange/green) so you can scan a batch of results at a glance

## How it works (short version)

You give it an IOC → it figures out what type it is → sends it to the right API → reads back the JSON response → applies some simple threshold logic (e.g. 5+ AV engines flagging something = malicious) → prints or writes the result.

## Tech used

Python, requests, openpyxl, AbuseIPDB API, VirusTotal API v3

## Why two versions?

I initially built the PowerShell version, but results only printed to the terminal — nothing was saved, and reviewing more than a few results at once was hard to track. That's when I had the idea to move the same logic into Excel, so results would persist and be easier to scan visually with color coding. Both versions are shown below to demonstrate that progression.

## Screenshots

Results from real test runs, both the PowerShell and Excel versions, across all supported IOC types.

### Batch check — mixed IPs, hashes, domains, and URLs in one run (PowerShell)
![Batch check PowerShell](https://github.com/Nasima-Begum/IOC-Lookup-Tool/blob/main/IOC-Powershell.png?raw=true)

### Email Header (Powershell)
![Email Header PowerShell](https://github.com/Nasima-Begum/IOC-Lookup-Tool/blob/main/Email-Powershell.png?raw=true)

### IP Sheet (Excel)
![IP Sheet](https://github.com/Nasima-Begum/IOC-Lookup-Tool/blob/main/IP.png?raw=true)

### Hash Sheet (Excel)
![Hash Sheet](https://github.com/Nasima-Begum/IOC-Lookup-Tool/blob/main/Hash.png?raw=true)

### Domain Sheet (Excel)
![Domain Sheet](https://github.com/Nasima-Begum/IOC-Lookup-Tool/blob/main/Domain.png?raw=true)

### URL Sheet (Excel)
![URL Sheet](https://github.com/Nasima-Begum/IOC-Lookup-Tool/blob/main/URL.png?raw=true)

### Email Headers Sheet (Excel)
![Email Headers Sheet](https://github.com/Nasima-Begum/IOC-Lookup-Tool/blob/main/Email.png?raw=true)
