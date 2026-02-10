---
name: blackops-center
description: Control multiple BlackOps Center sites from OpenClaw - create, publish, manage posts, generate heroes, and optimize SEO.
homepage: https://github.com/BlackOpsCenter/openclaw-skill
metadata: {"openclaw":{"emoji":"📝","requires":{"bins":["curl","jq"]}}}
---

# BlackOps Center Skill

Control your BlackOps Center sites from OpenClaw. Create, publish, and manage blog posts with advanced features for hero images, SEO, and multi-site support.

## Setup

1. **Generate an API token** in BlackOps Center:
   - Go to Settings → Browser Extension
   - Copy your Personal Access Token

2. **Configure the skill**:
   ```bash
   cd ~/.openclaw/skills/blackops-center
   cp config.example.yaml config.yaml
   # Edit config.yaml and paste your token
   ```

3. **Discover your sites**:
   ```bash
   blackops-center list-sites
   ```
   
   This will show all sites associated with your token. No manual configuration needed!

## Configuration

Create `config.yaml` with just your token:

```yaml
api_token: "your-token-here"
base_url: "https://blackopscenter.com"  # or your custom domain

# Optional: Set a default site (not recommended for multi-site users)
# default_domain: "yourdomain.com"
```

Sites are automatically discovered from your account. Run `list-sites` to see them.

**Important for Multi-Site Users:**
- Always specify `--domain` explicitly when running commands
- If you have multiple sites, do NOT set `default_domain` in config
- OpenClaw agents should ask which site to use if not specified in your request

## Available Commands

All commands use the `blackops-center` CLI wrapper.

### List Sites

Show all sites you have access to (auto-discovered from your token):

```bash
blackops-center list-sites
```

Returns JSON with all your sites:
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

No manual configuration needed — your token determines which sites you can access.

### List Posts

List posts for your site:

```bash
# List all posts
blackops-center list-posts

# List only published posts
blackops-center list-posts --status published

# List only drafts
blackops-center list-posts --status draft

# Limit results
blackops-center list-posts --limit 10
```

### Get a Post

Get full details of a specific post:

```bash
blackops-center get-post <post-id>
```

### Create a Post

Create a new draft post:

```bash
blackops-center create-post \
  --title "My Post Title" \
  --content "Post content in markdown" \
  --excerpt "Optional excerpt" \
  --tags "tag1,tag2,tag3"
```

All posts are created as drafts by default.

**Output:**
```
{"post": {...}, "site": {...}}  # JSON response on stdout

✓ Post created: draft
Preview: https://yoursite.com/preview/my-post-title?token=abc123
```

The command outputs JSON on stdout (for programmatic use) and a human-readable preview link on stderr.

### Update a Post

Update an existing post:

```bash
# Update title
blackops-center update-post <post-id> --title "New Title"

# Update content
blackops-center update-post <post-id> --content "New content"

# Publish a draft
blackops-center update-post <post-id> --status published

# Unpublish (back to draft)
blackops-center update-post <post-id> --status draft
```

You can combine multiple flags to update multiple fields at once.

**Output when publishing:**
```
{...}  # JSON response

✓ Post published
Live URL: https://yoursite.com/post-slug
```

**Output for draft updates:**
```
{...}  # JSON response

✓ Post updated: draft
Preview: https://yoursite.com/preview/post-slug?token=abc123
```

### Delete a Post

```bash
blackops-center delete-post <post-id>
```

### Generate Hero Image

Generate an AI-powered hero image for a post:

```bash
# Auto-generate from post content
blackops-center generate-hero --post-id abc123

# Custom prompt
blackops-center generate-hero --post-id abc123 \
  --prompt "futuristic AI assistant coding" \
  --style modern

# For specific site
blackops-center generate-hero --post-id abc123 \
  --domain example.com
```

**Styles:** modern, minimal, tech, artistic

### Update SEO Metadata

Update SEO fields for better search visibility:

```bash
# Update SEO title and description
blackops-center update-seo --post-id abc123 \
  --title "Best AI Tools for 2026" \
  --description "Comprehensive guide to AI automation tools"

# Update all SEO fields
blackops-center update-seo --post-id abc123 \
  --title "Complete Guide" \
  --description "Everything you need" \
  --keywords "ai,automation,productivity" \
  --og-image "https://example.com/hero.jpg"
```

### Manage Tags

Advanced tag management operations:

```bash
# Add tags to existing tags
blackops-center manage-tags --post-id abc123 \
  --action add --tags "ai,automation"

# Remove specific tags
blackops-center manage-tags --post-id abc123 \
  --action remove --tags "outdated,deprecated"

# Replace all tags
blackops-center manage-tags --post-id abc123 \
  --action replace --tags "new,fresh,updated"

# List current tags
blackops-center manage-tags --post-id abc123 --action list
```

### Send Tweet

Send a tweet or thread to Twitter:

```bash
# Single tweet (immediate)
blackops-center send-tweet \
  --text "Just shipped a new feature for BlackOps Center 🚀"

# Thread (immediate)
blackops-center send-tweet \
  --thread "1/ Just launched a major update..." \
  --thread "2/ New features include AI-powered hero images..." \
  --thread "3/ Check it out at blackopscenter.com"

# Scheduled tweet
blackops-center send-tweet \
  --text "Weekly content marketing tip 💡" \
  --scheduled-for "2026-02-10T14:30:00Z"

# Different site with scheduling
blackops-center send-tweet \
  --domain benenewton.com \
  --text "Personal update about my latest project" \
  --scheduled-for "2026-02-10T08:00:00-05:00"
```

**Options:**
- `--text TEXT` - Send a single tweet (either this or --thread required)
- `--thread TEXT` - Add a tweet to a thread (repeatable)
- `--domain DOMAIN` - Target specific site (uses default if not set)
- `--scheduled-for DATE` - Schedule for later (ISO 8601 format, must be future date)

**Scheduling:**
- Scheduled tweets are processed by the BlackOps Center cron job (runs every 5 minutes)
- Format: ISO 8601 with timezone (e.g., `2026-02-10T14:30:00Z` or `2026-02-10T09:30:00-05:00`)
- Must be a future date/time
- View scheduled tweets in the BlackOps Center Twitter dashboard

**Output (immediate):**
```
✓ Tweet posted
URL: https://x.com/BlackOpsTools/status/123456789
```

**Output (scheduled):**
```
✓ Tweet scheduled for 2026-02-10T14:30:00Z
```

### Reply to Tweet

Reply to a tweet on Twitter:

```bash
# Reply to a tweet by URL
blackops-center reply-tweet \
  --tweet-url "https://x.com/user/status/123456789" \
  --text "Great point! I wrote about this here: https://example.com/blog/post"

# Reply by tweet ID
blackops-center reply-tweet \
  --tweet-id "123456789" \
  --text "Exactly what I've been thinking 💯"

# Reply from specific site
blackops-center reply-tweet \
  --domain example.com \
  --tweet-url "https://x.com/user/status/123" \
  --text "This aligns with our research"
```

**Options:**
- `--text TEXT` - Reply text (required)
- `--tweet-url URL` - Tweet URL to reply to (either this or tweet-id required)
- `--tweet-id ID` - Tweet ID to reply to (either this or tweet-url required)
- `--domain DOMAIN` - Target specific site (uses default if not set)

## Multi-Site Support

All commands support `--domain` or `--site-id` flags to target specific sites:

```bash
# List posts for Your Site
blackops-center list-posts --domain yourdomain.com

# Create post on Another Site
blackops-center create-post --domain example.com \
  --title "Personal Post" --content "..."

# Generate hero for Third Site post
blackops-center generate-hero --post-id xyz789 \
  --domain anothersite.com --style tech
```

## Usage from OpenClaw

When you invoke this skill from a OpenClaw session, you can use natural language:

**User:** "Create a blog post about AI agents titled 'The Future of Automation'"

**Assistant will:**
1. **Ask which site** if you have multiple sites and didn't specify
2. Extract title and content from your message
3. Run `blackops-center create-post --domain yourdomain.com --title "..." --content "..."`
4. Return the post ID and preview URL

**User:** "Tweet from BlackOps Tools: 'Just shipped a new feature'"

**Assistant will:**
1. Identify the site from context (BlackOps Tools = blackopscenter.com)
2. Run `blackops-center send-tweet --domain blackopscenter.com --text "Just shipped a new feature"`
3. Return the posted tweet URL

**User:** "Reply to https://x.com/user/status/123 with 'Great insight!' from my personal site"

**Assistant will:**
1. Identify site from context (personal site = benenewton.com)
2. Run `blackops-center reply-tweet --domain benenewton.com --tweet-url "..." --text "Great insight!"`
3. Return the posted reply URL

**Important for Agents:**
- Always include `--domain` flag in commands
- If site is ambiguous, ask the user which site to use
- Use `list-sites` to show available options if needed

## API Details

This skill uses the BlackOps Center Extension API (`/api/ext/*`):

**Content Management:**
- `GET /api/ext/sites` - List sites
- `GET /api/ext/posts` - List posts
- `POST /api/ext/posts` - Create post
- `GET /api/ext/posts/:id` - Get post
- `PUT /api/ext/posts/:id` - Update post
- `DELETE /api/ext/posts/:id` - Delete post

**Advanced Features (v1.2.0+):**
- `POST /api/ext/posts/:id/generate-hero` - Generate AI hero image
- `PUT /api/ext/posts/:id/seo` - Update SEO metadata
- `POST /api/ext/posts/:id/tags` - Manage tags

**Twitter Integration:**
- `POST /api/ext/twitter/send` - Send tweet or thread
- `POST /api/ext/twitter/reply` - Reply to a tweet
- `POST /api/ext/twitter/generate-reply` - Generate AI reply text (UI only)

All requests require `Authorization: Bearer <token>` header.

**Multi-site routing:** All endpoints support `?domain=` or `?site_id=` query parameters.

## Error Handling

- **401 Unauthorized**: Token is invalid or revoked. Generate a new token in BlackOps Center.
- **404 Site not found**: The domain associated with your token doesn't exist.
- **404 Post not found**: Post ID doesn't exist or belongs to a different site.
- **400 Bad Request**: Missing required fields (e.g., title, content for create).

## Examples

### Create and publish workflow

```bash
# Create draft
POST_ID=$(blackops-center create-post \
  --title "My Post" \
  --content "# My Post\n\nGreat content here." | jq -r '.post.id')

# Review, edit if needed...

# Publish when ready
blackops-center update-post "$POST_ID" --status published
```

### Bulk operations

```bash
# Get all draft posts
DRAFTS=$(blackops-center list-posts --status draft)

# Publish all drafts (careful!)
echo "$DRAFTS" | jq -r '.posts[].id' | while read id; do
  blackops-center update-post "$id" --status published
done
```

## Troubleshooting

**"Unauthorized" error:**
- Verify your token in `config.yaml`
- Check token hasn't been revoked in BlackOps Center
- Generate a new token if needed

**"Site not found":**
- Each token is tied to a specific site domain
- If you need to manage multiple sites, generate separate tokens for each

**Command not found:**
- Make sure `bin/` is executable: `chmod +x ~/.openclaw/skills/blackops-center/bin/*`
- Skill should be installed via ClawdHub or symlinked to `~/.openclaw/skills/`

## Development

Test the API directly with curl:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
  https://blackopscenter.com/api/ext/posts
```

## Support

- BlackOps Center: https://blackopscenter.com
- Issues: https://github.com/openclaw/skills (if published)
- Documentation: This file
