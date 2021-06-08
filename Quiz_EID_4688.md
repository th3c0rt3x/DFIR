### Quiz - EID 4688
### cmd.exe /Q /c reg add HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest /v UseLogonCredential /t REG_DWORD /f /d 1 1> \\127.0.0.1\ADMIN$\__1622922177.2957754 2>&1
1. What's happening here?
2. What tool is it?
3. Detection ideas?
	
#### Answers:
An attacker performed remote WDigest auth provider *downgrade* attack to further dump users' clear text passwords from LSASS memory (newly logged on users). WDigest is disabled by default since Win 8.1 & WS 2012 R2

Note that WDigest registry key is not presented by default on some Windows OS versions, like WS2008 R2.

Here are some detection ideas for this activity:
* Look for specific cmdlines (cmd/powershell) aimed at changing "WDigest\UseLogonCredential" registry key value 0->1 (EID 4688, Sysmon EID 1, Powershell EID 400). "reg add" | set-itemproperty etc.
  * Note: don't use Powershell 4104, activity will be there, but only in Verbose logging level - it's too noisy.
			
* Look for "WDigest\UseLogonCredential" registry value change (0->1). Set SACL for the key and use Windows EID 4657 or use Sysmon EID 13. Use @SwiftOnSecurity Sysmon config file, it should be there.
			
* Look for Sysmon EID 17 (PipeCreate) and EID 18 (PipeConnect) events. Wmiexec uses specific named pipes naming scheme (check the source codeWinking face). Regular expressions will help you.
			
* Look for Windows EID 5145 (RelativeTargetName field) event with file share path ADMIN$/C$ etc and named pipes. Again, use the same regular expressions as used for PipeCreate/PipeConnect events.
	* (all these detection ideas may be implemented with free Sysmon and free Windows Audit EventLog events. You don't need EDRs to be able to detect this. There're also some ideas available with advanced EDR telemetry, but it's another talk)
			
* Oh, there are also network traffic signatures for wmiexec of course, but I'm *endpoint* hunter.
```
Courtesy : https://twitter.com/BlackMatter23/status/1401523637019189258
```
