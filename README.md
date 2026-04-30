# sysmon-config | A Sysmon configuration repository

This repository is a fork and extension of [@SwiftOnSecurity’s](https://twitter.com/SwiftOnSecurity) [sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config) and a modified version of Neo23x0’s [sysmon blocking config](https://github.com/Neo23x0/sysmon-config). It includes updated schema support, merged improvements, additional advanced detections, and optional blocking-focused builds.

The repository also includes two new **public Windows builds** designed for broader publication and testing:

- an **advanced public detection configuration**
- an **advanced public blocking configuration**

These public builds are intended to be more generic and easier to publish, without organization-specific exclusions or environment-specific tuning.

A Linux build is also available for testing.

## Version & Schema

The Windows configuration files in this repository are intended for **Sysmon 15.0 and newer**.  
Schema version is **4.90**.

> **Note:** The minimum supported Sysmon version for each configuration is documented inside the XML header comments. Please verify compatibility against the Sysmon version you are deploying.

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
- Malicious LOLDrivers blocking and detection builds
- Expanded advanced detections across multiple Sysmon event types
- Linux Sysmon detection coverage for supported event classes

## Configs in this Repository

This repository includes the original configs, Linux configs, and multiple advanced Windows builds for both detection and blocking use cases.

- `sysmonconfig-advanced-public-detection-v2.xml`  
  Public-facing advanced **detection-only** Windows configuration. This build is designed for broader use and publication, with environment-specific exclusions removed. It includes advanced detections such as suspicious process creation, process tampering, file deletion detection, expanded LOLBin coverage, and vulnerable or malicious driver **detection** using Event ID 29.  
  **Important:** Event ID 29 detects **PE file creation** on disk and is useful for identifying vulnerable or malicious drivers being dropped to a system. It does **not** detect driver load by itself.

- `sysmonconfig-advanced-public-block-v2.xml`  
  Public-facing advanced Windows configuration with **blocking enabled**. This build includes the same advanced detection logic as the detection-only version, but also adds **blocking of malicious LOLDrivers by hash** using Event ID 27.  
  **Important:** Event ID 27 blocks **PE file creation** on disk by hash match. It is useful for preventing known malicious driver files and other malicious PE files from being written, but it is **not** a substitute for driver load monitoring. This file is intended for users who want a more aggressive, prevention-oriented configuration and should be tested carefully before broad deployment.

- `sysmonconfig-Linux-advanced.xml`  
  Purpose-built detection configuration for Linux hosts running Sysmon for Linux (schema 4.81), covering the event types supported by the Linux agent, including process creation, network connections, process termination, raw access reads, file creation, and file deletion. Forked from [Microsoft](https://github.com/microsoft/SysmonForLinux) and extended with advanced detections.

- `sysmonconfig-Linux.xml`  
  Linux configuration for Sysmon for Linux forked from [Microsoft](https://github.com/microsoft/SysmonForLinux) with additional detections.

- `sysmonconfig-export.xml`  
  The original “OG” configuration based on the config provided by @SwiftOnSecurity.

- `sysmonconfig-export-block-loldrivers.xml`  
  A Windows blocking configuration merged with [@magicsword-io LOLDrivers](https://github.com/magicsword-io/LOLDrivers/blob/main/detections/sysmon/sysmon_config_malicious_hashes_block.xml) to block malicious Living Off The Land Drivers using Event ID 27.  
  **Important:** This uses file creation blocking logic and should be validated carefully before production deployment.

- `sysmonconfig-export-block.xml`  
  The original blocking-oriented config provided by @Neo23x0, with additional advanced blocking rules available in newer Sysmon versions. **Use with care.**

- `sysmonconfig-malicious-hashes-exe-detect.xml`  
  A Windows detection-focused configuration that builds upon Neo23x0’s config by incorporating newer Sysmon features and merging [@magicsword-io LOLDrivers](https://github.com/magicsword-io/LOLDrivers/blob/main/detections/sysmon/sysmon_config_malicious_hashes_exe_detect.xml) for Event ID 29. This is similar to `sysmonconfig-export-block-loldrivers.xml` but is **detection only** and does not block.  
  **Important:** This detects suspicious or known bad PE file creation events and is not the same as driver load telemetry.

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

1. Expressions that generate a high volume of events
2. Blocking issues or missing exclusions
3. Broken configuration elements such as typos or invalid conditions
4. Missing detection coverage, preferably in the form of a pull request

## Usage 

### Install 

Run with administrator rights

batch
sysmon.exe -accepteula -i sysmonconfig-export.xml

## Update existing configuration 

Run with administrator rights

batch
sysmon.exe -c sysmonconfig-export.xml

## Uninstall 

Run with administrator rights

batch
sysmon.exe -u
