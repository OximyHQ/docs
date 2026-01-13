# Troubleshooting Guide

## Technical Documentation Source for Customer Docs

---

## Device Enrollment Issues

### Enrollment Code Expired

**Symptoms:**
- Code shows "Expired" in dashboard
- Sensor app rejects the code

**Cause:**
Enrollment codes expire after 5 minutes for security.

**Solution:**
1. In the dashboard, click "Generate new token"
2. Enter the new code within 5 minutes
3. Make sure device time is correctly set

---

### Device Not Connecting

**Symptoms:**
- Dashboard shows "Waiting for device to connect..."
- Sensor app shows connecting state
- No progress after entering code

**Possible Causes & Solutions:**

**1. Network Issues**
- Check internet connection on the device
- Try a different network (not behind corporate proxy)
- Ensure firewall isn't blocking Oximy

**2. Wrong Code Entered**
- Verify code matches exactly (6 digits)
- Check for typos (0 vs O, 1 vs l)
- Note: codes are formatted with space for readability (e.g., "158 221")

**3. Sensor App Issues**
- Quit and restart the sensor app
- Check if app is fully installed
- Verify permissions were granted

---

### Device Shows Offline After Enrollment

**Symptoms:**
- Device enrolled successfully
- Status shows "Offline" immediately

**Possible Causes & Solutions:**

**1. Sensor App Not Running**
- Check menu bar (Mac) or system tray (Windows)
- Open Oximy app if not running
- Check if app was set to launch at startup

**2. Computer Went to Sleep**
- Wake the computer
- Verify network reconnects
- Status should update to "Online"

**3. Network Change**
- If network changed, sensor may need to reconnect
- Check internet connectivity

---

## Activity Detection Issues

### No Activities Showing

**Symptoms:**
- Home page shows empty activity feed
- Using AI tools but nothing appears

**Possible Causes & Solutions:**

**1. Sensor Not Running**
- Check that sensor app is active
- Look for Oximy in menu bar/system tray

**2. Permissions Not Granted**
- Mac: Check System Preferences → Security & Privacy
- Ensure Accessibility and Network permissions granted
- May need to re-grant after OS update

**3. Unsupported Tool**
- Tool may not be in supported list
- Click "Missing an app?" to request support
- Check Inventory for supported tools

**4. Using Incognito/Private Mode**
- Some browsers in private mode may not be detected
- Try using regular browsing mode

---

### Activities Delayed

**Symptoms:**
- Activities appear after a delay
- Expected real-time but seeing lag

**Possible Causes:**
- Network latency
- High activity volume
- Dashboard refresh timing

**Solution:**
Activities typically appear within a few seconds. If delays persist beyond 30 seconds, check network connection.

---

### Wrong Tool Showing

**Symptoms:**
- Activity shows incorrect tool name
- Tool not recognized correctly

**Solution:**
1. Use "Missing an app?" feature
2. Submit details about the tool
3. Team will investigate and improve detection

---

## Dashboard Access Issues

### Can't Sign In

**Symptoms:**
- Sign-in page not loading
- Getting authentication errors

**Possible Causes & Solutions:**

**1. Incorrect Credentials**
- Verify email address is correct
- Try "Forgot password" to reset
- Check for caps lock

**2. Account Not Verified**
- Check email for verification link
- Check spam folder
- Request new verification email

**3. Browser Issues**
- Clear browser cache and cookies
- Try incognito/private window
- Try a different browser

---

### Dashboard Not Loading

**Symptoms:**
- White screen or loading forever
- Error messages in browser

**Possible Causes & Solutions:**

**1. Browser Compatibility**
- Use Chrome (recommended), Firefox, Safari, or Edge
- Update to latest browser version

**2. Network Issues**
- Check internet connection
- Try refreshing the page
- Clear browser cache

**3. JavaScript Blocked**
- Ensure JavaScript is enabled
- Check for browser extensions blocking scripts

---

### Session Expired

**Symptoms:**
- Suddenly logged out
- Actions fail silently

**Solution:**
1. Refresh the page
2. Sign in again if prompted
3. Sessions expire for security after inactivity

---

## Inventory Issues

### Tools Not Appearing

**Symptoms:**
- Know a tool is being used
- Doesn't show in Inventory

**Possible Causes & Solutions:**

**1. Tool Not Yet Detected**
- Make sure the tool has actually been used
- Check Home page for recent activity
- May take a moment for first detection

**2. Tool Not Supported**
- Click "Missing an app?" to request support
- Provide tool name and how it's accessed

**3. Search Filter Active**
- Clear the search box
- Check if filters are applied

---

### Search Not Finding Tool

**Symptoms:**
- Know tool exists
- Search returns no results

**Solution:**
- Search checks: name, domain, provider
- Try different search terms
- Tool may be listed under different name

---

## Ask Feature Issues

### Questions Not Working

**Symptoms:**
- Ask returns no results
- Error messages when asking

**Possible Causes & Solutions:**

**1. No Data Available**
- Ask requires some activity data to query
- Wait for activities to accumulate

**2. Query Too Complex**
- Try simpler questions
- Use suggested questions as examples

**3. API Issues**
- System may fall back to local parsing
- Retry the question
- Some features may be limited in fallback mode

---

### Incorrect Results

**Symptoms:**
- Ask returns unexpected data
- Numbers don't match expectations

**Solution:**
- Results are based on available data
- Check time range of data
- Some metrics may be approximations

---

## Theme/UI Issues

### Theme Not Saving

**Symptoms:**
- Theme resets on page refresh
- Dark/light mode doesn't persist

**Solution:**
- Ensure cookies are enabled
- Clear browser data and retry
- Theme preference stored locally

---

### Keyboard Shortcuts Not Working

**Symptoms:**
- Pressing shortcuts does nothing
- Navigation keys not responding

**Possible Causes & Solutions:**

**1. Focus in Input Field**
- Click outside text inputs
- Shortcuts disabled when typing

**2. Modal Open**
- Close any open modals/drawers first
- Press Escape to close

**3. Browser Shortcut Conflict**
- Some browsers override shortcuts
- Check browser extension conflicts

---

## Performance Issues

### Slow Dashboard Loading

**Symptoms:**
- Pages take a long time to load
- UI feels sluggish

**Possible Causes & Solutions:**

**1. Large Data Volume**
- Dashboard optimized for most use cases
- Very large datasets may take longer

**2. Network Speed**
- Check internet connection
- Try on faster network

**3. Browser Performance**
- Close unused tabs
- Restart browser
- Clear cache

---

### Activity Feed Lagging

**Symptoms:**
- Activities slow to appear
- Feed not updating

**Solution:**
- Refresh the page
- Check sensor is running
- Network may be cause

---

## Device Management Issues

### Can't Delete Device

**Symptoms:**
- No delete option visible
- Delete action fails

**Solution:**
- Navigate to device detail page
- Look for device management options
- Contact support if needed

---

### Device Name Not Updating

**Symptoms:**
- Changed device name
- Old name still showing

**Solution:**
- Changes should save immediately
- Try refreshing the page
- Check for error messages

---

## Settings Issues

### Settings Not Saving

**Symptoms:**
- Click save but nothing happens
- Success message doesn't appear

**Possible Causes & Solutions:**

**1. No Changes Made**
- Save button disabled if no changes
- Make a change first

**2. Validation Error**
- Check for error messages
- Ensure valid values selected

**3. Network Issue**
- Check connection
- Retry saving

---

### Wrong Timezone Showing

**Symptoms:**
- Times displayed incorrectly
- Expected different timezone

**Solution:**
1. Go to Settings
2. Select correct timezone from dropdown
3. Save changes
4. All times will update

---

## Getting Help

### Reporting Issues

If problems persist:

1. **Note the symptoms**: What exactly is happening?
2. **Steps to reproduce**: What actions led to the issue?
3. **Screenshots**: Visual evidence helps
4. **Browser info**: Browser name and version
5. **Contact support**: Use Help & Support in sidebar

### Support Channels

- **In-app**: Help & Support popup
- **Email**: support@oximy.com
- **GitHub**: For sensor-related issues (open source)

---

## Quick Fixes Summary

| Problem | Quick Fix |
|---------|-----------|
| Code expired | Generate new code |
| Device offline | Check sensor app is running |
| No activities | Check permissions granted |
| Can't sign in | Reset password |
| Dashboard blank | Clear cache, try different browser |
| Tool missing | Request via "Missing an app?" |
| Wrong times | Change timezone in Settings |
| Shortcuts not working | Click outside input fields |

---

## Common Error Messages

### "Authentication required"

**Meaning**: Session expired or not signed in
**Fix**: Sign in again

### "Network error"

**Meaning**: Couldn't reach server
**Fix**: Check internet connection

### "Token expired"

**Meaning**: Enrollment code timed out
**Fix**: Generate new enrollment code

### "Permission denied"

**Meaning**: Action not allowed
**Fix**: May need admin access; contact workspace admin

### "Tool not found"

**Meaning**: Requested tool doesn't exist
**Fix**: Check tool name, may not be supported yet
