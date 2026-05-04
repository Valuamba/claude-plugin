# Security Checklist

## Secrets

- [ ] No hardcoded API keys, tokens, passwords in any file
- [ ] Sensitive values use `userConfig` with `"sensitive": true`
- [ ] `.gitignore` excludes `.env`, credentials, and local config files
- [ ] Scripts read secrets from environment variables, not arguments

## Input Validation

- [ ] URLs validated: only http/https allowed
- [ ] `file://` protocol blocked
- [ ] Localhost and private IPs blocked (127.0.0.1, 10.x, 172.16-31.x, 192.168.x)
- [ ] Cloud metadata endpoints blocked (169.254.169.254)
- [ ] File paths sanitized (no `../` traversal)
- [ ] User input escaped before shell execution
- [ ] No `eval()` on user input
- [ ] No string interpolation into shell commands

## Network

- [ ] All HTTP requests have timeouts (max 30s)
- [ ] Response body size limited
- [ ] TLS/SSL verified (no `verify=False`)
- [ ] User-Agent header set

## Scripts

- [ ] No `os.system()` or `subprocess.shell=True` with user input
- [ ] Arguments passed as lists to subprocess, not concatenated strings
- [ ] Temp files cleaned up
- [ ] Error messages don't leak sensitive paths or tokens

## Hooks

- [ ] Hook scripts are executable but not world-writable
- [ ] Hook commands don't pass secrets via CLI arguments
- [ ] Hook output doesn't contain secrets

## MCP Servers

- [ ] Server processes run with minimal privileges
- [ ] API tokens passed via env vars, not args
- [ ] Server validates all incoming tool calls
