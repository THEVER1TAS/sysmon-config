# sysmon-config | Advanced Sysmon configuration repository

This repository is a fork and extension of [@SwiftOnSecurity’s](https://twitter.com/SwiftOnSecurity) [sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config) and a modified version of Neo23x0’s [sysmon blocking config](https://github.com/Neo23x0/sysmon-config).

It includes:

- updated schema support
- merged improvements from upstream and community work
- additional advanced detections
- optional blocking-focused builds
- Linux Sysmon configurations for testing and development

The repository also includes two new Windows builds intended for broader publication and testing:

- **advanced public detection**
- **advanced public blocking**

These builds are designed to be more generic and easier to share, without organization-specific exclusions or environment-specific tuning.

> The older Windows builds previously listed in this README — `sysmonconfig-export.xml`, `sysmonconfig-export-block-loldrivers.xml`, `sysmonconfig-export-block.xml`, and `sysmonconfig-malicious-hashes-exe-detect.xml` — have been superseded by the new advanced builds below.

## Version & Schema

The Windows configuration files in this repository are intended for **Sysmon 15.0 and newer**.  
Schema version is **4.90**.

> **Note:** Minimum supported Sysmon versions may vary by configuration. Check the XML header comments in each file before deployment.

## Maintainer of this Fork

- VER1TAS [@THE_VER1TAS](https://twitter.com/THE_VER1TAS)

## Maintainers of Neo23x0 Fork

- Florian Roth [@Neo23x0](https://twitter.com/cyb3rops)
- Tobias Michalski [@humpalum](https://twitter.com/_humpalum)
- Christian Burkard [@phantinuss](https://twitter.com/phantinuss)
- Nasreddine Bencherchali [@nas_bench](https://twitter.com/nas_bench)

## Additional Coverage Includes

- Cobalt Strike named pipes
- Sliver implants
- PrintNightmare
- HiveNightmare
- malicious LOLDrivers blocking and detection builds
- expanded advanced detections across multiple Sysmon event types
- Linux Sysmon detection coverage for supported event classes

## Configs in this Repository

This repository includes Linux configs and the current Windows builds for both detection and blocking use cases.

- `sysmonconfig-advanced-public-detection-v2.xml`  
  Advanced **detection-only** Windows configuration built for broad deployment and sharing, with environment-specific exclusions removed. This build adds expanded detections for suspicious process creation, process tampering, file deletion activity, LOLBin abuse, and vulnerable or malicious driver **detection** using Event ID 29. It incorporates detection logic derived from [@magicsword-io LOLDrivers](https://github.com/magicsword-io/LOLDrivers/blob/main/detections/sysmon/sysmon_config_malicious_hashes_exe_detect.xml).  
  **Important:** Event ID 29 detects **PE file creation** on disk and helps identify vulnerable or malicious drivers being dropped to a system. It does **not** detect driver load by itself.

- `sysmonconfig-advanced-public-block-v2.xml`  
  Advanced Windows configuration with **blocking enabled** for more aggressive prevention-oriented deployments. This build includes the same advanced detection logic as the detection-only version, while also adding **blocking of malicious LOLDrivers by hash** using Event ID 27. It incorporates blocking logic derived from [@magicsword-io LOLDrivers](https://github.com/magicsword-io/LOLDrivers/blob/main/detections/sysmon/sysmon_config_malicious_hashes_block.xml).  
  **Important:** Event ID 27 blocks **PE file creation** on disk by hash match. It helps prevent known malicious driver files and other malicious PE files from being written, but it is **not** a substitute for driver load monitoring. This file should be tested carefully before broad deployment.

- `sysmonconfig-Linux-advanced.xml`  
  Purpose-built detection configuration for Linux hosts running Sysmon for Linux (schema 4.81), covering the event types supported by the Linux agent, including process creation, network connections, process termination, raw access reads, file creation, and file deletion. Forked from [Microsoft](https://github.com/microsoft/SysmonForLinux) and extended with advanced detections.

- `sysmonconfig-Linux.xml`  
  Linux configuration for Sysmon for Linux forked from [Microsoft](https://github.com/microsoft/SysmonForLinux) with additional detections.

## Driver Monitoring and Blocking Notes

Several configurations in this repository use **Event ID 27** and **Event ID 29** for malicious or vulnerable driver-related coverage.

- **Event ID 27** = file creation **blocking**
- **Event ID 29** = file creation **detection**

These events are valuable for identifying or preventing known bad PE files, including malicious or vulnerable drivers, when they are **written to disk**.

They do **not** replace:

- `DriverLoad` monitoring with **Event ID 6**
- Microsoft’s vulnerable driver blocklist
- broader endpoint protection or EDR controls

If you want visibility into drivers that are actually being loaded into the kernel, you should also review your `DriverLoad` coverage and OS security policy settings.

## Other Sysmon Configs

- Olaf Hartong’s [Sysmon Modular](https://github.com/olafhartong/sysmon-modular) — modular Sysmon configuration framework for easier maintenance and generation of specific configs

## Testing

These configurations focus on advanced detection coverage and optional blocking behavior. Community testing is welcome and appreciated.

Please report:

1. expressions that generate a high volume of events
2. blocking issues or missing exclusions
3. broken configuration elements such as typos or invalid conditions
4. missing detection coverage, preferably in the form of a pull request

## Usage

### Install

Run with administrator rights

```batch
sysmon.exe -accepteula -i sysmonconfig-advanced-public-detection-v2.xml
```

### Update existing configuration

Run with administrator rights

```batch
sysmon.exe -c sysmonconfig-advanced-public-detection-v2.xml
```

### Uninstall

Run with administrator rights

```batch
sysmon.exe -u
```
