# Troubleshooting Reference

> **Purpose**: Technical reference for creating customer-facing troubleshooting documentation. Contains common issues, diagnostics steps, and solutions for both macOS and Windows.

---

## Quick Diagnostics Checklist

Before deep troubleshooting, verify these basics:

| Check | macOS | Windows |
|-------|-------|---------|
| App is running | Menu bar icon visible | System tray icon visible |
| Certificate installed | Keychain Access > "Oximy CA" | certmgr.msc > Trusted Root CAs |
| Proxy enabled | System Preferences > Network > Proxies | Internet Options > LAN Settings |
| mitmproxy running | Activity Monitor > "python" | Task Manager > "python.exe" |
| Port accessible | `lsof -i :1030` | `netstat -an \| findstr 1030` |

---

## Certificate Issues

### Certificate Not Trusted

**Symptoms**:
- SSL/TLS errors in applications
- "NET::ERR_CERT_AUTHORITY_INVALID" in browsers
- "Certificate not trusted" warnings

**macOS Solutions**:

1. **Verify installation**:
   - Open Keychain Access
   - Search for "Oximy CA"
   - Certificate should appear in System or login keychain

2. **Check trust settings**:
   - Double-click the Oximy CA certificate
   - Expand "Trust" section
   - "When using this certificate" should be "Always Trust"

3. **Reinstall certificate**:
   - In Oximy app: Settings > Certificate > Toggle off then on
   - This regenerates and reinstalls the certificate
   - You'll be prompted for your password

**Windows Solutions**:

1. **Verify installation**:
   - Press Win+R, type `certmgr.msc`
   - Navigate to Trusted Root Certification Authorities > Certificates
   - Look for "Oximy CA"

2. **Check correct store**:
   - Certificate should be in LocalMachine\Root for system-wide trust
   - If only in CurrentUser\Root, some apps may not trust it

3. **Reinstall with elevation**:
   - Run Oximy as Administrator
   - Go to Settings > Certificate > Uninstall, then Install again
   - UAC prompt should appear

### Certificate Generation Failed

**macOS**:
- Error: "Failed to generate CA certificate"
- **Cause**: OpenSSL execution failed or disk write error
- **Solution**:
  - Check disk space in home directory
  - Ensure `~/.oximy/` is writable
  - Try: `mkdir -p ~/.oximy && chmod 755 ~/.oximy`

**Windows**:
- Error: "Failed to generate certificate"
- **Cause**: .NET cryptography exception or disk write error
- **Solution**:
  - Check disk space
  - Ensure `%USERPROFILE%\.oximy\` is writable
  - Check antivirus isn't blocking file creation

### Certificate Files Missing

**Location Check**:
- macOS: `~/.oximy/mitmproxy-ca.pem` and `~/.oximy/mitmproxy-ca-cert.pem`
- Windows: `%USERPROFILE%\.oximy\mitmproxy-ca.pem` and `mitmproxy-ca-cert.pem`

**If missing**:
1. Toggle certificate off in Settings
2. Toggle certificate on (regenerates files)
3. Files should be recreated

---

## Proxy Issues

### Proxy Not Working

**Symptoms**:
- Traffic not being captured
- No events appearing in traces
- Apps connecting directly (bypassing proxy)

**macOS Diagnostics**:

1. **Check proxy status**:
   ```bash
   networksetup -getsecurewebproxy Wi-Fi
   ```
   Should show:
   ```
   Enabled: Yes
   Server: 127.0.0.1
   Port: 1030
   ```

2. **Test proxy manually**:
   ```bash
   curl -x http://127.0.0.1:1030 https://api.openai.com/v1/models -v
   ```

3. **Check all network services**:
   ```bash
   networksetup -listallnetworkservices
   ```
   Proxy should be set on all active services.

**Windows Diagnostics**:

1. **Check registry**:
   ```cmd
   reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyEnable
   reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyServer
   ```
   Should show ProxyEnable=1 and ProxyServer=127.0.0.1:PORT

2. **Check Internet Options**:
   - Open Internet Options (inetcpl.cpl)
   - Connections tab > LAN settings
   - Proxy server should be checked with 127.0.0.1:PORT

3. **Test proxy**:
   ```cmd
   curl -x http://127.0.0.1:1030 https://api.openai.com/v1/models
   ```

### Port Already in Use

**Symptoms**:
- "Port 1030 already in use" error
- Proxy fails to start

**macOS**:
```bash
# Find what's using the port
lsof -i :1030

# Kill if it's a stuck Oximy process
kill -9 PID
```

**Windows**:
```cmd
# Find what's using the port
netstat -ano | findstr :1030

# Kill the process
taskkill /PID XXXX /F
```

**Solution**:
- Restart Oximy app
- App will find next available port (1031, 1032, etc.)
- Check Settings > Advanced for current port number

### Proxy Enabled But Traffic Not Captured

**Possible Causes**:

1. **Application using its own proxy settings**:
   - Some apps ignore system proxy (Node.js, Python scripts)
   - Solution: Configure app to use HTTP_PROXY environment variable

2. **Application using certificate pinning**:
   - Some apps validate server certificates strictly
   - Solution: These apps cannot be intercepted (by design)

3. **mitmproxy process crashed**:
   - Check if python process is running
   - Restart Oximy app

4. **OISP bundle not loaded**:
   - Bundle defines which APIs to capture
   - Check Settings > Detection Bundle > Refresh

---

## mitmproxy Process Issues

### mitmproxy Won't Start

**Symptoms**:
- "Failed to start proxy service" error
- No python process running

**macOS Checks**:
1. Check python-embed directory exists in app bundle
2. Check addon.py is present
3. Look at logs: `~/.oximy/logs/`

**Windows Checks**:
1. Check `Resources\python-embed\python.exe` exists
2. Check `Resources\oximy-addon\addon.py` exists
3. Look at logs: `%USERPROFILE%\.oximy\logs\`

**Common Causes**:
- Antivirus blocking Python execution
- Corrupted installation
- Missing Python dependencies

**Solution**: Reinstall Oximy app

### mitmproxy Crashes Repeatedly

**Symptoms**:
- Proxy keeps stopping
- "Max restart attempts reached" error

**Diagnostics**:
1. Check log files for error messages
2. Look for Python tracebacks
3. Check if specific traffic patterns cause crash

**Common Causes**:
- Memory exhaustion (very large responses)
- Malformed HTTP traffic
- Python addon error

---

## Application-Specific Issues

### Browser Not Using Proxy

**Chrome/Edge**:
- Usually respects system proxy automatically
- Try: Close all browser windows, restart browser
- Check: chrome://net-internals/#proxy

**Firefox**:
- Has its own proxy settings
- Go to: Settings > Network Settings
- Select "Use system proxy settings"

**Safari (macOS)**:
- Uses system proxy by default
- Should work automatically

### Electron Apps (Slack, Discord, VS Code)

- Some Electron apps have proxy configuration
- Check app settings for proxy options
- Set to use system proxy if available

### Terminal/CLI Tools

Many CLI tools don't use system proxy by default:

**Configure environment variables**:
```bash
# macOS/Linux
export HTTP_PROXY=http://127.0.0.1:1030
export HTTPS_PROXY=http://127.0.0.1:1030

# Windows (Command Prompt)
set HTTP_PROXY=http://127.0.0.1:1030
set HTTPS_PROXY=http://127.0.0.1:1030

# Windows (PowerShell)
$env:HTTP_PROXY = "http://127.0.0.1:1030"
$env:HTTPS_PROXY = "http://127.0.0.1:1030"
```

---

## Data & Sync Issues

### No Events Being Captured

**Check List**:
1. Is proxy enabled? (Check app status)
2. Is mitmproxy running? (Check process list)
3. Is the app you're using actually making AI API calls?
4. Is the OISP bundle loaded? (Check Settings > Detection Bundle)

**Test with curl**:
```bash
# macOS
curl -x http://127.0.0.1:1030 -k https://api.openai.com/v1/chat/completions \
  -H "Authorization: Bearer test" \
  -d '{"model":"gpt-4","messages":[{"role":"user","content":"hi"}]}'
```

**Check traces file**:
- macOS: `cat ~/.oximy/traces/traces_$(date +%Y-%m-%d).jsonl | wc -l`
- Windows: Check file in `%USERPROFILE%\.oximy\traces\`

### Events Not Syncing

**Symptoms**:
- "X events pending sync" message
- Events captured locally but not appearing in dashboard

**Causes**:
1. Not logged in to workspace
2. Network connectivity issues
3. API endpoint unreachable
4. Invalid device token

**Solutions**:
1. Check login status in Settings > Account
2. Check internet connection
3. Try "Sync Now" button (macOS)
4. Log out and log back in to refresh token

### Traces Folder Empty

**If no files**:
- Proxy may not be capturing
- Check mitmproxy process is running
- Check OISP bundle is loaded

**If files exist but empty**:
- Traffic not matching OISP patterns
- Test with known AI API call

---

## Update Issues

### Updates Not Installing (macOS)

**Sparkle Update Issues**:
1. Check internet connection
2. Check "Automatic Updates" is enabled in Settings
3. Try "Check for Updates" manually
4. Check Console.app for Sparkle errors

### Updates Not Installing (Windows)

**Velopack Update Issues**:
1. Check internet connection
2. Check GitHub Releases accessible
3. Check download progress in Settings
4. Check logs for update errors

---

## Reset & Recovery

### Soft Reset (Keep Data)

1. Quit Oximy app
2. Relaunch app
3. App reinitializes services

### Hard Reset (Clear Settings)

**macOS**:
1. Settings > Advanced > "Reset All Settings"
2. OR manually delete UserDefaults:
   ```bash
   defaults delete com.oximy.mac
   ```

**Windows**:
1. Settings > Log Out
2. Manually delete app settings if needed

### Complete Reset (Clear Everything)

**macOS**:
```bash
# Quit app first
rm -rf ~/.oximy/
defaults delete com.oximy.mac
# Remove from Keychain via Keychain Access
# Reinstall app
```

**Windows**:
```cmd
# Quit app first
rmdir /s /q %USERPROFILE%\.oximy
# Remove certificate via certmgr.msc
# Reinstall app
```

---

## Diagnostic Commands Reference

### macOS

```bash
# Check if Oximy is running
pgrep -fl Oximy

# Check python/mitmproxy process
pgrep -fl python | grep mitmproxy

# Check port usage
lsof -i :1030

# Check proxy settings
networksetup -getsecurewebproxy Wi-Fi
networksetup -getwebproxy Wi-Fi

# Check certificate
security find-certificate -a -c "Oximy CA" -p

# View logs
cat ~/.oximy/logs/*.log

# View recent traces
tail -10 ~/.oximy/traces/traces_$(date +%Y-%m-%d).jsonl | jq .

# Test proxy connection
curl -x http://127.0.0.1:1030 -v https://api.openai.com/
```

### Windows

```cmd
# Check if Oximy is running
tasklist | findstr Oximy

# Check python process
tasklist | findstr python

# Check port usage
netstat -ano | findstr :1030

# Check proxy settings
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings"

# Check certificate (PowerShell)
Get-ChildItem Cert:\LocalMachine\Root | Where-Object {$_.Subject -like "*Oximy*"}

# View logs
type %USERPROFILE%\.oximy\logs\*.log

# Test proxy connection
curl -x http://127.0.0.1:1030 -v https://api.openai.com/
```

---

## Getting Help

### Information to Collect

When contacting support, include:
1. Operating system and version
2. Oximy app version
3. Error messages (screenshots or text)
4. Steps to reproduce
5. Relevant log excerpts

### Log Locations

| Platform | Path |
|----------|------|
| macOS | `~/.oximy/logs/` |
| Windows | `%USERPROFILE%\.oximy\logs\` |

### Support Channels

- Email: support@oximy.com
- Documentation: https://docs.oximy.com
- GitHub Issues: https://github.com/oximyhq/sensor/issues

---

## Known Limitations

### Apps That Cannot Be Intercepted

Some applications implement certificate pinning and will reject the Oximy CA:
- Banking/financial apps
- Some security tools
- Apps with custom TLS validation

**Behavior**: These apps may fail to connect or show certificate errors when proxy is enabled.

**Solution**: Add these apps to bypass list (future feature) or disable proxy temporarily.

### Traffic Types Not Captured

- Non-HTTP traffic (raw TCP/UDP)
- Traffic to localhost (bypassed by default)
- Traffic from apps that bypass system proxy
- Encrypted traffic to non-standard ports (without explicit config)
