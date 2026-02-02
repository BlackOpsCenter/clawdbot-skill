# BlackOps Center Skill for OpenClaw

Control your [BlackOps Center](https://blackopscenter.com) content from OpenClaw. Create, publish, and manage blog posts with advanced features for hero images, SEO optimization, and multi-site support.

## What is BlackOps Center?

BlackOps Center is a Content Operating System for operators, founders, and technical leaders. It helps you turn ideas into outcomes through persistent memory, discovery bots, content generation, and analytics.

## What This Skill Does

This skill lets you interact with your BlackOps Center sites directly from OpenClaw:

- **Multi-site management** - Control all your BlackOps sites from one place
- **List and search posts** across all your sites
- **Create new posts** from conversation or direct commands
- **Publish drafts** with a simple command
- **Update existing content** without opening the web interface
- **Generate hero images** with AI (new in v1.2.0)
- **Optimize SEO** - manage titles, descriptions, keywords, and OG images (new in v1.2.0)
- **Advanced tag management** - add, remove, replace tags programmatically (new in v1.2.0)
- **Delete posts** when needed

Perfect for:
- Voice-driven content creation (via Your Site + OpenClaw + BlackOps)
- Automated publishing workflows across multiple sites
- SEO optimization and content enhancement
- Quick post status changes
- Content audits and inventory
- Building custom automation on top of your content

## Installation

### Via ClawdHub (recommended)

```bash
clawdhub install blackops-center
```

### Manual Installation

1. Clone or download this skill to `~/.openclaw/skills/blackops-center`
2. Copy `config.example.yaml` to `config.yaml`
3. Generate an API token in BlackOps Center (Settings → Browser Extension)
4. Paste your token into `config.yaml`

## Quick Start

1. **Get your API token:**
   - Log into BlackOps Center
   - Go to Settings → Browser Extension
   - Click "Regenerate Token" (or use existing if you have one)
   - Copy the token

2. **Configure the skill:**
   ```bash
   cd ~/.openclaw/skills/blackops-center
   cp config.example.yaml config.yaml
   nano config.yaml  # paste your token
   ```

3. **Discover your sites:**
   ```bash
   blackops-center list-sites
   ```
   
   This automatically shows all sites accessible with your token. No manual site configuration needed!

4. **Test it:**
   ```bash
   blackops-center list-posts --status draft
   blackops-center list-posts --domain yourdomain.com
   ```

4. **Use with OpenClaw:**
   - "Show me my recent draft posts in BlackOps"
   - "Create a blog post titled 'AI Automation in 2026'"
   - "Publish post abc123"

## Commands

All commands are available via the `blackops-center` CLI:

### `list-sites`
Show all sites you have access to (auto-discovered from your token).

```bash
blackops-center list-sites
```

**Output:**
```json
{
  "sites": [
    {
      "id": "abc-123",
      "domain": "yourdomain.com",
      "name": "Your Site Name"
    }
  ]
}
```

Your token determines which sites you can access — no manual configuration needed.

### `list-posts`
List posts with optional filters.

```bash
blackops-center list-posts
blackops-center list-posts --status draft
blackops-center list-posts --status published --limit 10
```

### `get-post <id>`
Get full details of a specific post.

```bash
blackops-center get-post abc123-def456
```

### `create-post`
Create a new draft post.

```bash
blackops-center create-post \
  --title "My Post Title" \
  --content "Post content in markdown" \
  --excerpt "Optional excerpt" \
  --tags "ai,automation,productivity"
```

**New in v1.1.0:** Automatically outputs shareable preview link:
```
✓ Post created: draft
Preview: https://yoursite.com/preview/my-post-title?token=abc123
```

### `update-post <id>`
Update an existing post (including publishing).

```bash
# Publish a draft
blackops-center update-post abc123 --status published

# Update content
blackops-center update-post abc123 --content "New content"

# Update multiple fields
blackops-center update-post abc123 \
  --title "New Title" \
  --status published
```

**New in v1.1.0:** Shows published or preview URL after update:
```
✓ Post published
Live URL: https://yoursite.com/post-slug
```

### `delete-post <id>`
Delete a post (permanent).

```bash
blackops-center delete-post abc123
```

### `generate-hero` (New in v1.2.0)
Generate an AI-powered hero image for a post.

```bash
# Auto-generate from post content
blackops-center generate-hero --post-id abc123

# Custom prompt and style
blackops-center generate-hero --post-id abc123 \
  --prompt "AI assistant helping developer" \
  --style modern
```

### `update-seo` (New in v1.2.0)
Update SEO metadata for better search visibility.

```bash
# Update SEO title and description
blackops-center update-seo --post-id abc123 \
  --title "Best AI Tools 2026" \
  --description "Complete guide to AI automation"

# Update all SEO fields including keywords and OG image
blackops-center update-seo --post-id abc123 \
  --title "Complete Guide" \
  --description "Everything you need" \
  --keywords "ai,automation,tools" \
  --og-image "https://example.com/hero.jpg"
```

### `manage-tags` (New in v1.2.0)
Advanced tag management operations.

```bash
# Add tags
blackops-center manage-tags --post-id abc123 \
  --action add --tags "ai,automation"

# Remove tags
blackops-center manage-tags --post-id abc123 \
  --action remove --tags "outdated"

# Replace all tags
blackops-center manage-tags --post-id abc123 \
  --action replace --tags "new,fresh"

# List current tags
blackops-center manage-tags --post-id abc123 --action list
```

## Usage with OpenClaw

Once installed, OpenClaw can use this skill when you mention BlackOps Center in your requests:

**Example conversations:**

> **You:** "Create a blog post about the future of AI agents on Your Site"
> 
> **OpenClaw:** [Creates draft post on yourdomain.com, returns post ID and preview URL]

> **You:** "Show me all my draft posts on example.com"
> 
> **OpenClaw:** [Lists drafts from example.com with titles, IDs, and creation dates]

> **You:** "Publish post abc123"
> 
> **OpenClaw:** [Updates status to published, confirms with live URL]

> **You:** "Generate a hero image for post xyz789 on Third Site with a modern tech style"
> 
> **OpenClaw:** [Generates AI hero image, returns URL]

> **You:** "Update SEO for post abc123 with title 'Best AI Tools' and description 'Complete guide'"
> 
> **OpenClaw:** [Updates SEO metadata, confirms success]

## Configuration

The `config.yaml` file supports:

```yaml
api_token: "your-token-here"     # Required: Your BlackOps Center API token
base_url: "https://blackopscenter.com"  # Optional: Custom domain if self-hosted

# Multi-site configuration (v1.2.0+)
sites:
  - id: "site-uuid-1"
    domain: "yourdomain.com"
    name: "Your Site"
    default: true
    
  - id: "site-uuid-2"
    domain: "example.com"
    name: "Another Site"
    
  - id: "site-uuid-3"
    domain: "anothersite.com"
    name: "Third Site"
```

## Multi-Site Support (v1.2.0+)

The skill now supports managing multiple sites from a single configuration:

1. **Configure sites in config.yaml** - Add all your sites with their IDs and domains
2. **Use --domain flag** - Target specific sites: `--domain example.com`
3. **Use --site-id flag** - Or target by ID: `--site-id abc-123-def`
4. **Set a default** - Mark one site as default in the config

All commands (create, update, list, generate-hero, update-seo, manage-tags) support multi-site routing.

## API Reference

This skill uses the BlackOps Center Extension API:

**Content Management:**
- `GET /api/ext/sites` - List accessible sites
- `GET /api/ext/posts` - List posts (with filters)
- `POST /api/ext/posts` - Create post
- `GET /api/ext/posts/:id` - Get post
- `PUT /api/ext/posts/:id` - Update post
- `DELETE /api/ext/posts/:id` - Delete post

**Advanced Features (v1.2.0+):**
- `POST /api/ext/posts/:id/generate-hero` - Generate AI hero image
- `PUT /api/ext/posts/:id/seo` - Update SEO metadata
- `POST /api/ext/posts/:id/tags` - Manage tags (add/remove/replace)

All endpoints require `Authorization: Bearer <token>` header.

**Multi-site routing:** Use `?domain=example.com` or `?site_id=uuid` query parameters.

## Troubleshooting

**"Unauthorized" error:**
- Verify your token in `config.yaml`
- Check if token was revoked in BlackOps Center
- Generate a new token

**"Site not found":**
- Your token is tied to a specific site domain
- Verify the domain in BlackOps Center settings

**Command not found:**
- Ensure scripts are executable: `chmod +x ~/.openclaw/skills/blackops-center/bin/*`
- Check skill is installed in `~/.openclaw/skills/`

**JSON parsing errors:**
- Ensure `jq` is installed: `brew install jq` (macOS) or `apt install jq` (Linux)

## Requirements

- OpenClaw (any recent version)
- BlackOps Center account with active site
- `jq` for JSON processing (usually pre-installed)
- `curl` for API requests (standard on macOS/Linux)

## Contributing

This skill is part of the BlackOps Center ecosystem. Issues and improvements:

- Skill issues: [ClawdHub repository](https://github.com/openclaw/skills)
- BlackOps API issues: Contact support@blackopscenter.com
- Feature requests: Use the feedback form in BlackOps Center

## License

MIT License - See LICENSE file for details

## Links

- [BlackOps Center](https://blackopscenter.com)
- [OpenClaw](https://openclaw.io)
- [ClawdHub](https://clawdhub.com)
- [Your Site](https://yourdomain.com) - Voice-first idea capture (pairs great with this workflow)
