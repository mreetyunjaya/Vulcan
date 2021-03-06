# Vulcan

Tool to extract various things from `.nessus` files.
At the moment it does common service URIs (`--services`), SMB shares (`--shares`), SMB share permissions (`--sharepermissions`), and vulnerabilities (`--listvulnerabilities` and `--listallvulnerabilities`).

Services can optionally be filtered to just http[s] using `--urls`.

In all cases, FQDNs can be included, where present, by specifying `--fqdns`

Verbose output is written to `stderr`, so useful output can be piped directly to file, other tools, or the clipboard.

## Installation

`git clone --recurse-submodules https://github.com/sysophost/Vulcan`

**remember to use `--recurse-submodules` to ensure you pick up the `pyShot` submodule**

## Usage

`python vulcan.py --inputfile <input .nessus file> [--urls] [--shares] [--sharepermissions] [--screenshot] [--fqdns]`

### Required args

`--inputfile` / `-if`

Path to the input `.nessus` file to parse
<br><br><br>
**You will need to specify at least one of `--services`, `--urls`, `--shares`, `--sharepermissions` or `--listvulnerabilities`**

### Optional args

`--services` / `-sv`

Extract all services identified by the `Service Detection` plugin in *unauthenticated* scans

`--urls` / `-u`

Only extract http[s] URIs from the extracted services

`--shares` / `-sh`

Extract SMB shares identified by the `Microsoft Windows SMB Shares Enumeration` in *authenticated* scans

`--sharepermissions` / `-sp`

Extract SMB share permissions identified by the `Microsoft Windows SMB Share Permissions Enumeration` in *authenticated* scans

`--listvulnerabilities` / `-lv`

List all vulnerabilties in the supplied `.nessus` file and group by host (ordered by severity in descending order)

`--listallvulnerabilities` / `-lva`

List unique vulnerabilities in the supplied `.nessus` file, and order by severity in descending order.

`--minseverity` / `-ns`

Set minimum severity level filter (0-4). Default=1

`--maxseverity` / `-xs`

Set maximum severity level filter (0-4). Default=4

* 0=Info
* 1=Low
* 2=Medium
* 3=High
* 4=Critical

`--fqdns` / `-f`

Output FQDN instead of IP address (where one exists)

`--screenshot` / `-s`

Capture screenshots of identified http[s] services

`--outputdir` / `-od`

Output directory for screenshots

*If this doesn't exist it will be created*

`--proxy` /  `-p`

Proxy to use for outgoing screenshot connections

This currently supports `HTTP` and `SOCKS4/5`

## Examples

### Extract all http[s] endpoints and open in firefox

`python vulcan.py --inputfile <input .nessus file> --services --urls [--fqdns]  | xargs firefox`

*This assumes that `firefox` is on the path*

## TODO

* Work out what to do with services that are not identified by Nessus
* Handle hosts with multple FQDNs
* Design a better data structure to hold a mapping between Nessus service names and the associated URI
* ~~Take screenshots of http[s] URIs~~
