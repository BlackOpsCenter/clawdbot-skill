# BlackOps Center v1.2.0 - Quick Reference

## Multi-Site Commands

All commands support `--domain <domain>` or `--site-id <uuid>` for site targeting.

### Content Management

```bash
# List posts for specific site
blackops-center list-posts --domain example.com

# Create post on specific site
blackops-center create-post --domain yourdomain.com \
  --title "Title" --content "Content"

# Update post
blackops-center update-post <post-id> --status published

# Delete post
blackops-center delete-post <post-id>
```

### Hero Image Generation (v1.2.0)

```bash
# Auto-generate from post content
blackops-center generate-hero --post-id <id>

# Custom prompt with style
blackops-center generate-hero --post-id <id> \
  --prompt "AI assistant coding" \
  --style modern

# For specific site
blackops-center generate-hero --post-id <id> \
  --domain example.com --style tech
```

**Styles:** modern | minimal | tech | artistic

### SEO Optimization (v1.2.0)

```bash
# Update SEO title and description
blackops-center update-seo --post-id <id> \
  --title "SEO Title" \
  --description "Meta description"

# Full SEO update with keywords and OG image
blackops-center update-seo --post-id <id> \
  --title "Title" \
  --description "Description" \
  --keywords "tag1,tag2,tag3" \
  --og-image "https://example.com/image.jpg"

# Single field update
blackops-center update-seo --post-id <id> \
  --description "New description only"
```

### Tag Management (v1.2.0)

```bash
# Add tags (append to existing)
blackops-center manage-tags --post-id <id> \
  --action add --tags "new,tags"

# Remove specific tags
blackops-center manage-tags --post-id <id> \
  --action remove --tags "old,outdated"

# Replace all tags
blackops-center manage-tags --post-id <id> \
  --action replace --tags "fresh,new,updated"

# List current tags
blackops-center manage-tags --post-id <id> --action list
```

**Actions:** add | remove | replace | list

## Sites Configuration

Add to `config.yaml`:

```yaml
sites:
  - id: "uuid-1"
    domain: "yourdomain.com"
    name: "Your Site"
    default: true
    
  - id: "uuid-2"
    domain: "example.com"
    name: "Another Site"
    
  - id: "uuid-3"
    domain: "anothersite.com"
    name: "Third Site"
```

## Common Workflows

### Create → Generate Hero → Optimize SEO → Publish

```bash
# 1. Create draft
POST_ID=$(blackops-center create-post \
  --title "My Post" \
  --content "# Content" \
  --domain example.com | jq -r '.post.id')

# 2. Generate hero
blackops-center generate-hero --post-id $POST_ID --style modern

# 3. Optimize SEO
blackops-center update-seo --post-id $POST_ID \
  --title "Optimized Title" \
  --description "SEO description" \
  --keywords "ai,automation,tools"

# 4. Add tags
blackops-center manage-tags --post-id $POST_ID \
  --action add --tags "featured,trending"

# 5. Publish
blackops-center update-post $POST_ID --status published
```

### Update Existing Post

```bash
# Get post
blackops-center get-post <post-id> --domain yourdomain.com

# Update content
blackops-center update-post <post-id> \
  --content "Updated content"

# Refresh hero
blackops-center generate-hero --post-id <post-id> --style minimal

# Update SEO
blackops-center update-seo --post-id <post-id> \
  --description "Updated description"
```

### Bulk Operations

```bash
# List all drafts on Third Site
DRAFTS=$(blackops-center list-posts \
  --domain anothersite.com \
  --status draft)

# Process each draft
echo "$DRAFTS" | jq -r '.posts[].id' | while read id; do
  echo "Processing $id"
  blackops-center generate-hero --post-id "$id" --style tech
  blackops-center manage-tags --post-id "$id" --action add --tags "automated"
done
```

## API Endpoints

**Content:**
- `GET /api/ext/sites` - List sites
- `GET /api/ext/posts` - List posts
- `POST /api/ext/posts` - Create post
- `GET /api/ext/posts/:id` - Get post
- `PUT /api/ext/posts/:id` - Update post
- `DELETE /api/ext/posts/:id` - Delete post

**Advanced (v1.2.0):**
- `POST /api/ext/posts/:id/generate-hero` - Hero generation
- `PUT /api/ext/posts/:id/seo` - SEO update
- `POST /api/ext/posts/:id/tags` - Tag management

**Multi-site routing:** Add `?domain=<domain>` or `?site_id=<uuid>` to any endpoint.

## Tips

- Use `jq` for JSON parsing in scripts
- Output JSON to stdout, messages to stderr
- Chain commands with `$()` or pipes
- Set default site in config.yaml for convenience
- Use `--help` on any command for detailed options

## Version History

- **v1.2.0** (2026-02-02): Multi-site, hero gen, SEO, tags
- **v1.1.0** (2026-02-01): Preview URLs, better output
- **v1.0.0** (2026-01-29): Initial release
