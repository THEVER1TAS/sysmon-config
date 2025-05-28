# sysmon-config | A Sysmon configuration file
This is a forked and modified version of [@SwiftOnSecurity's](https://twitter.com/SwiftOnSecurity) [sysmon config](https://github.com/SwiftOnSecurity/sysmon-config) and
a modified version of Neo23x0's [sysmon blocking config](https://github.com/Neo23x0/sysmon-config). This includes all pull requests, updated schema, and additional blocking rules. The `sysmonconfig-export-block-loldrivers.xml` builds upon Neo23x0's config by also including a curated blocking list of malicious [Living Off The Land Windows](https://www.loldrivers.io/) drivers used by adversaries to bypass security controls and carry out attacks. A Linux build is now available for testing!

## Version & Schema
The configuration files are for Sysmon 15.0 and newer. Schema version is 4.90 and the binary version is now 18. 

## Maintainers of this Fork
- VER1TAS [@THE_VER1TAS](https://twitter.com/THE_VER1TAS)

## Maintainers of Neo23x0 Fork
- Florian Roth [@Neo23x0](https://twitter.com/cyb3rops)
- Tobias Michalski [@humpalum](https://twitter.com/_humpalum)
- Christian Burkard [@phantinuss](https://twitter.com/phantinuss)
- Nasreddine Bencherchali [@nas_bench](https://twitter.com/nas_bench)
  
## Additional coverage includes
- Cobalt Strike named pipes
- Sliver Implants
- PrinterNightmare
- HiveNightmare

## Configs in this Repository
This repo includes the original and three additional configurations

- `sysmonconfig-Linux.xml` Linux config for Sysmon forked from [Microsoft](https://github.com/microsoft/SysmonForLinux) with addiitonal detections.
- `sysmonconfig-export.xml` the "OG" config provided by @SwiftOnSecurity
- `sysmonconfig-export-block-loldrivers.xml` merged with [@magicsword-io LOLDrivers](https://github.com/magicsword-io/LOLDrivers/blob/main/detections/sysmon/sysmon_config_malicious_hashes_block.xml) to block Living Off The Land   Drivers (Malicious Hashes Only) using EventID 27.
- `sysmonconfig-export-block.xml` the original config provided by @Neo23x0 with some additional advanced blocking rules available since Sysmon v14 (WARNING: use it with care!)
- `sysmonconfig-malicious-hashes-exe-detect.xml` builds upon @Neo23x0's config by incorporating the newest features in Sysmon 15.0 and merging [@magicsword-io LOLDrivers](https://github.com/magicsword-io/LOLDrivers/blob/main/detections/sysmon/sysmon_config_malicious_hashes_exe_detect.xml) for EventID 29. This is similar to the `sysmonconfig-export-block-loldrivers.xml` config but does not include blocking, only detection.

## Other Sysmon Configs
- Olaf Hartong's [Sysmon Modular](https://github.com/olafhartong/sysmon-modular) - modular Sysmon config for easier maintenance and generation of specific configs

## Testing
This configuration is focused on detection coverage and blocking rules. I need help with testing this configuration, so please reach out!

Please report:

1. Expressions that cause a high volume of events
2. Blocking issues or missing elements
3. Broken configuration elements (typos, wrong conditions)
4. Missing coverage (preferrably as a pull request)

## Usage

### Install

Run with administrator rights

```batch
sysmon.exe -accepteula -i sysmonconfig-export.xml
```

### Update existing configuration

Run with administrator rights

```batch
sysmon.exe -c sysmonconfig-export.xml
```

### Uninstall

Run with administrator rights

```batch
sysmon.exe -u
```
