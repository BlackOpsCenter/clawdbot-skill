# Changelog

All notable changes to the BlackOps Center skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
