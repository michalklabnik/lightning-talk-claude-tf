---
name: test-deployment
description: Test deployed VM web server functionality using Playwright MCP browser automation
allowed-tools: Task, Bash, Read
user-invocable: true
argument-hint: [optional: IP address or path to tofu directory]
---

# Test Deployed Web Server via Playwright MCP

This skill verifies that a deployed VM with web server is working correctly.

## When to Use

After successful `tofu apply`, when you want to verify functionality.

## Workflow

### 1. Get IP Address

If not provided as argument, get IP from OpenTofu output:

```bash
tofu output -raw public_ip
```

### 2. Wait for Nginx

Before testing, verify that nginx is running. Use curl in a loop:

```bash
for i in {1..24}; do
  curl -s -o /dev/null -w "%{http_code}" --max-time 5 http://IP && break
  echo "Attempt $i - waiting..."
  sleep 5
done
```

**Important:**
- Maximum wait: 2 minutes (24 attempts × 5 seconds)
- Check HTTP response code
- Timeout per attempt: 5 seconds

### 3. Test via Playwright MCP

Use AGENT with Playwright MCP to:

1. Navigate to `http://IP`
2. Take full page screenshot
3. Save screenshot to `./outputs/` directory

**Required:**
- Use Task agent with general-purpose subagent type
- Agent should use Playwright MCP tools
- Screenshot must be saved for presentation
- Screenshot filename should include timestamp

### 4. Analyze Result

After saving screenshot, verify:

- Is QR code visible?
- Is URL text "klabnik.link/talk-claude-tf" visible?
- Does page load completely?
- Are all images/assets loaded?

## Expected Outcome

- Screenshot saved in `./outputs/`
- Confirmation that page contains QR code and URL
- HTTP 200 response code
- Nginx successfully serving content

## Error Handling

If nginx is not responding after 2 minutes:
- Check VM status in Azure portal
- Review cloud-init logs: `sudo cat /var/log/cloud-init-output.log`
- Check nginx status: `sudo systemctl status nginx`

## Example Usage

```
/test-deployment
```

or with explicit IP:

```
/test-deployment 20.123.45.67
```

## Notes

- Everything is done via AGENT - no custom code needed
- Use MCP tools for browser automation
- Screenshot must be saved for presentation/verification
