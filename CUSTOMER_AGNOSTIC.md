# Customer-Agnostic Design

## Overview

The BlackOps Center skill is designed to work for **any BlackOps Center customer** without hardcoded references to specific sites, domains, or UUIDs.

## How It Works

1. **Token-Based Authentication**
   - Customer generates their own API token in BlackOps Center
   - Token grants access to all sites they manage
   - No site-specific configuration needed

2. **Auto-Discovery**
   - Sites are discovered via `GET /api/ext/sites` API endpoint
   - Token determines which sites the user can access
   - No manual site list configuration required

3. **Per-Command Site Selection**
   - Commands accept `--domain` or `--site-id` flags
   - Optional `default_domain` in config for convenience
   - Falls back to user prompt if ambiguous

## Configuration

Minimal config required:

```yaml
api_token: "user-provided-token"
base_url: "https://blackopscenter.com"  # or custom instance

# Optional default
default_domain: "example.com"
```

## Site Discovery Workflow

```bash
# 1. User enters token in config.yaml
# 2. Discover their sites
blackops-center list-sites

# Output shows their sites (auto-discovered):
{
  "sites": [
    {"id": "abc-123", "domain": "site1.com", "name": "Site 1"},
    {"id": "def-456", "domain": "site2.com", "name": "Site 2"}
  ]
}

# 3. Use commands with site selection
blackops-center list-posts --domain site1.com
blackops-center create-post --domain site2.com --title "Post"
```

## No Hardcoded References

✅ **What IS included:**
- Generic command structure
- API endpoint patterns
- Example placeholders like "yourdomain.com"

❌ **What is NOT included:**
- Specific customer site UUIDs
- Specific customer domains
- Preset site configurations

## Examples in Documentation

All examples use:
- Generic placeholders: `yourdomain.com`, `example.com`
- Descriptive names: `"Your Site Name"`
- Placeholder UUIDs: `abc-123`, `def-456`

Real customer domains (voicecommit.com, benenewton.com, vitalwall.com) were removed from v1.3.0 to avoid confusion.

## Deployment

The skill can be:
- Published to ClawHub for any customer to install
- Shared via GitHub for open-source contribution
- Distributed without any customer-specific modifications

## Verification

To verify customer-agnostic design:

```bash
# Check config.example.yaml - should have no real domains/IDs
cat config.example.yaml

# Check documentation - should use generic examples
grep -r "benenewton\|voicecommit\|vitalwall" *.md

# Should return no matches (or only in this file)
```

## Support

Each customer gets the same skill with the same features. Site access is determined entirely by their API token permissions in BlackOps Center.
