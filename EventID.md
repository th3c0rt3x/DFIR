
### Useful Event ID's
| # | Description |  Notes |
|---|-------------|---|
|  4648 |      Logon using explicit credentials (RunAs)       |   |
|  4671 |       Account logon with superuser rights (Administrator)      |   |
|Service|
|7034	|Service crashed unexpectedly| Services can crash due to attacks like process injection|
|7035	|Service sent a Start/Stop control| |
|7036	|Service started or stopped| |
|7040	|Start type changed (Boot / On Request / Disabled)| |
|7045	|A new service was installed on the system (Win2008R2+)| |
|4697	|A new service was installed on the system (Security log)| * A large amount of malware and worms in the wild utilize Services <br> * Services started on boot illustrate persistence (desirable in malware)|


What all are logon event-id's that can record using traditional methods WMI or Sysmon
* When backdoors installed
* Exploited Services
