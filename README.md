# sysmon-config | Advanced Sysmon Configuration Repository

This repository contains advanced Microsoft Sysmon configurations focused on practical endpoint telemetry, detection engineering, threat hunting, and SIEM enrichment.

This project started as a fork and extension of the excellent community work from [@SwiftOnSecurity](https://twitter.com/SwiftOnSecurity), [Neo23x0 / Nextron Systems](https://github.com/Neo23x0/sysmon-config), and other contributors in the Sysmon community. My goal with this fork is to continue building on that work by adding current detection logic, practical tuning, and configurations that can help organizations improve visibility across Windows and Linux endpoints.

These configurations are designed to help security teams collect high-value telemetry for investigation, detection engineering, and incident response. They are not intended to be deployed blindly. Every environment is different, and organizations should test, tune, and validate these files against their own EDR, SIEM, endpoint management tools, business applications, and operational requirements.

## Project Focus

This repository focuses on:

- high-value Sysmon telemetry for Windows and Linux systems
- advanced Windows detection-only and blocking configurations
- LOLDrivers / MagicSword hash coverage for malicious and vulnerable driver activity
- improved detection coverage for suspicious process, registry, DNS, file, pipe, image load, and driver activity
- practical SIEM use cases for Splunk, Microsoft Sentinel, Elastic, and other platforms
- configurations that can be adapted and tuned by defenders for their own environments

## Version & Schema

The Windows configuration files are intended for **Sysmon 15.0 and newer**.

Current Windows schema version:

```text
4.90
```

Minimum supported Sysmon versions may vary by configuration. Review the XML header comments before deployment.

## Maintainer of This Fork

- VER1TAS [@THE_VER1TAS](https://twitter.com/THE_VER1TAS)

## Credits and Acknowledgements

This project would not exist without the work of the broader detection engineering and Sysmon community.

Special credit to:

- [@SwiftOnSecurity](https://twitter.com/SwiftOnSecurity) — original Sysmon baseline configuration
- Florian Roth [@Neo23x0](https://twitter.com/cyb3rops) / Nextron Systems — blocking config and advanced detection work
- Tobias Michalski [@humpalum](https://twitter.com/_humpalum)
- Christian Burkard [@phantinuss](https://twitter.com/phantinuss)
- Nasreddine Bencherchali [@nas_bench](https://twitter.com/nas_bench)
- [@magicsword-io LOLDrivers](https://github.com/magicsword-io/LOLDrivers) — malicious and vulnerable driver hash intelligence
- [Microsoft Sysmon for Linux](https://github.com/microsoft/SysmonForLinux) — Linux Sysmon support
- Olaf Hartong’s [Sysmon Modular](https://github.com/olafhartong/sysmon-modular) — modular Sysmon configuration framework
- The many researchers, defenders, and contributors who continue to improve Sysmon detection quality

## What This Repository Adds

Additional coverage includes:

- malicious LOLDrivers detection and blocking options
- vulnerable driver drop detection
- Cobalt Strike named pipe coverage
- Sliver implant indicators
- PrintNightmare-related detections
- HiveNightmare-related detections
- expanded LOLBin and proxy-execution coverage
- suspicious browser, Office, PowerShell, and script interpreter behavior
- process tampering visibility
- file deletion visibility
- advanced Linux Sysmon detection coverage

## Configurations in This Repository

This repository includes current Windows and Linux configurations for different deployment and testing use cases.

### Windows Configurations

#### `sysmonconfig-advanced-public-detection-v3.3.xml`

Advanced **detection-only** Windows configuration built for broad deployment, sharing, and customization.

This configuration is intended for organizations that want improved Windows endpoint visibility without enabling Sysmon blocking behavior. It includes detections for suspicious process creation, LOLBin abuse, browser and Office child-process activity, PowerShell abuse, process tampering, file deletion activity, DNS activity, registry persistence, named pipes, and malicious LOLDrivers executable drops.

This configuration uses **Event ID 29** for malicious LOLDrivers file creation detection.

**Important:** Event ID 29 detects matching PE file creation on disk. It does not block the file and does not replace driver load monitoring.

#### `sysmonconfig-advanced-public-block-v3.3.xml`

Advanced Windows configuration with **blocking enabled** for organizations that want a more aggressive prevention-oriented Sysmon configuration.

This configuration includes the same advanced detection logic as the detection-only build, while also using **Event ID 27** to block known malicious LOLDrivers hashes. It also uses **Event ID 29** to detect vulnerable driver executable drops.

This configuration is intended for careful testing before production deployment.

**Important:** Event ID 27 blocks matching PE file creation by hash. Event ID 29 detects matching PE file creation. These events are useful for malicious and vulnerable driver file activity, but they do not replace DriverLoad telemetry or operating system vulnerable driver protections.

### Linux Configurations

#### `sysmonconfig-Linux-advanced.xml`

Advanced Linux Sysmon configuration intended to provide high-signal telemetry for common Linux post-exploitation behaviors.

This configuration is designed to help defenders identify suspicious Linux activity such as process execution from temporary locations, shell and interpreter activity, suspicious network tooling, file creation in high-risk paths, raw access behavior, and other activity that may support threat hunting or incident response.

This is intended as a practical Linux endpoint telemetry baseline that can be adapted to different Linux server and workstation environments.

#### `sysmonconfig-Linux.xml`

Base Linux Sysmon configuration forked from [Microsoft Sysmon for Linux](https://github.com/microsoft/SysmonForLinux) with additional detection logic.

## Driver Monitoring and Blocking Notes

Several configurations in this repository use Event ID 27 and Event ID 29 for malicious or vulnerable driver-related coverage.

- **Event ID 27** = file creation blocking
- **Event ID 29** = file creation detection

These events are useful for identifying or preventing known malicious or vulnerable PE files when they are written to disk.

They do **not** replace:

- `DriverLoad` monitoring with Event ID 6
- Microsoft’s vulnerable driver blocklist
- EDR prevention controls
- kernel-mode security protections
- broader endpoint detection and response coverage

Organizations should continue to monitor driver load activity and maintain operating system, EDR, and vulnerable driver protections.

## Deployment Guidance

These configurations are intended to be tested before broad rollout.

Before deploying at scale, review and tune for:

- EDR / antivirus products
- SIEM forwarders
- endpoint management tools
- software deployment agents
- business applications
- developer tools
- browser installation paths
- expected administrative activity
- known high-volume internal tooling

The public Windows configurations intentionally avoid private or organization-specific exclusions. Add your own environment-specific exclusions only after validating the source of noise.

## Usage

### Install

Run with administrator rights:

```batch
sysmon.exe -accepteula -i sysmonconfig-advanced-public-detection-v3.3.xml
```

For the blocking configuration:

```batch
sysmon.exe -accepteula -i sysmonconfig-advanced-public-block-v3.3.xml
```

### Update Existing Configuration

Run with administrator rights:

```batch
sysmon.exe -c sysmonconfig-advanced-public-detection-v3.3.xml
```

For the blocking configuration:

```batch
sysmon.exe -c sysmonconfig-advanced-public-block-v3.3.xml
```

### Uninstall

Run with administrator rights:

```batch
sysmon.exe -u
```

## Testing and Feedback

Testing, feedback, and pull requests are welcome.

Please report:

1. rules that generate high event volume
2. false positives or missing exclusions
3. broken or invalid configuration elements
4. missing detection coverage
5. useful improvements for Windows or Linux telemetry
6. validation results from different environments

When reporting an issue, please include:

- Sysmon version
- operating system version
- configuration file used
- relevant Event ID
- RuleName if available
- sample event details with sensitive data removed

## Other Sysmon Projects

- [SwiftOnSecurity sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config)
- [Neo23x0 / Nextron Systems sysmon-config](https://github.com/Neo23x0/sysmon-config)
- [Olaf Hartong Sysmon Modular](https://github.com/olafhartong/sysmon-modular)
- [Microsoft Sysmon for Linux](https://github.com/microsoft/SysmonForLinux)
- [MagicSword LOLDrivers](https://github.com/magicsword-io/LOLDrivers)

## Final Note

Sysmon is not a replacement for EDR, application control, vulnerability management, or secure system configuration. It is a powerful telemetry source that becomes most valuable when paired with a SIEM, strong detection content, and an organization-specific tuning process.

Use these configurations as a starting point, validate them in your own environment, and continue improving them based on real telemetry.
