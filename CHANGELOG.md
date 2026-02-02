# Changelog

All notable changes to the BlackOps Center skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.3.0] - 2026-02-02

### Changed
- **Simplified configuration** - Removed manual site array from config.yaml
- Sites are now **auto-discovered** via `list-sites` API call using your token
- Users only need to provide their API token in config
- Optional `default_domain` or `default_site_id` for convenience
- Updated all documentation to reflect token-only setup workflow

### Benefits
- Zero manual site configuration required
- Works immediately after entering token
- No hardcoded site IDs or domains in examples
- Fully customer-agnostic deployment

## [1.2.0] - 2026-02-02

### Added
- **Multi-site support**: Configure multiple sites (VoiceCommit, benenewton.com, VitalWall) in `config.yaml`
- **Hero image generation**: New `generate-hero` command for AI-powered hero image creation
- **SEO management**: New `update-seo` command for managing SEO metadata (title, description, keywords, OG image)
- **Tag management**: New `manage-tags` command for advanced tag operations (add, remove, replace, list)
- Enhanced config structure with per-site configuration (id, domain, name, default flag)
- Support for `--domain` and `--site-id` flags across all commands for multi-site routing

### Changed
- Updated config.example.yaml with multi-site configuration examples
- Enhanced help text to show advanced features and multi-site examples
- Improved documentation for all commands with multi-site usage patterns

### Technical Details
- All new commands integrate with BlackOps Center Extension API
- Hero generation supports custom prompts and style selection
- SEO updates support partial field updates
- Tag management supports atomic operations (add/remove/replace)

## [1.1.0] - 2026-02-01

### Added
- **Preview URL output**: `create-post` now automatically outputs a shareable preview link after creating drafts
- **Published URL output**: `update-post` shows live URL when publishing posts, preview URL for drafts
- Human-readable status messages on stderr (JSON remains on stdout for backward compatibility)

### Changed
- Improved user experience with immediate access to preview/published URLs
- Better feedback when creating or updating posts

### Technical Details
- Messages go to stderr to maintain script composability (JSON on stdout)
- Supports multi-site routing via `--domain` parameter
- Preview tokens extracted from API response and formatted as clickable URLs

## [1.0.1] - 2026-01-30

### Fixed
- Initial bug fixes and stability improvements

## [1.0.0] - 2026-01-29

### Added
- Initial release
- Support for creating, listing, updating, and deleting posts
- Multi-site support via API token
- Draft and published status management
- Tag support
- Featured image support
- Markdown content handling

### Commands
- `list-sites` - Show accessible sites
- `list-posts` - List posts with filters (status, limit)
- `get-post` - Get full post details
- `create-post` - Create new draft posts
- `update-post` - Update existing posts
- `delete-post` - Delete posts

[1.1.0]: https://github.com/BlackOpsCenter/clawdbot-skill/compare/v1.0.1...v1.1.0
[1.0.1]: https://github.com/BlackOpsCenter/clawdbot-skill/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/BlackOpsCenter/clawdbot-skill/releases/tag/v1.0.0
