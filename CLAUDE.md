# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains two main components of the **otu.ai** ecosystem:

1. **otu.ai.web**: Next.js-based web application with mobile support (AI-powered smart memo application)
2. **otu.ai.cli**: Command-line interface tool for memo management

**Current Version**: 0.5.201 (web), CLI version tracked separately

### Working with CLI Project

When working on `otu.ai.cli`, use `otu.ai.web` as a reference for:
- Database schema and models
- API endpoints and integration patterns
- Authentication flows
- Supabase configuration
- Error handling patterns
- Environment variable setup

## CLI Project Development

### Technology Stack

- **Runtime**: Node.js with ES modules
- **CLI Framework**: Commander.js
- **Database**: Supabase (PostgreSQL) - shared with web app
- **Authentication**: OAuth via Supabase Auth
- **Storage**: File-based authentication tokens

### Key Commands

```bash
# Development
node otu.js [command] [options]

# Available commands
node otu.js login                    # OAuth authentication
node otu.js logout                   # Clear authentication
node otu.js create "title"           # Create new memo
node otu.js create -f file.txt       # Create memo from file
node otu.js list --limit 5           # List memos with pagination
node otu.js view --id [uuid]         # View specific memo
node otu.js view [index]             # View by index (latest first)
```

### Environment Setup

```bash
cd otu.ai.cli
npm install

# Environment setup
cp .env.template .env.local
# Configure Supabase variables (reference web app's .env.template)

# Start local Supabase (if needed)
npx supabase start
```

### Required Environment Variables

```bash
# Supabase (required - reference web app for values)
SUPABASE_URL=
SUPABASE_ANON_KEY=
SUPABASE_AUTH_GITHUB_CLIENT_ID=
SUPABASE_AUTH_GITHUB_SECRET=

# Optional
SUPABASE_SERVICE_ROLE_KEY=  # For admin operations
```

## Reference Information from Web App

When updating CLI, reference these patterns from `otu.ai.web`:

### Database Schema

Reference `otu.ai.web/supabase/migrations/` for current schema:
- `page` table structure
- User management tables
- Authentication tables
- Index patterns

### API Patterns

Reference `otu.ai.web/app/api/` for:
- Authentication patterns
- Error handling format
- Response structure
- Rate limiting
- Usage tracking patterns

### Authentication Flow

Reference web app's auth system:
- OAuth provider setup
- Token handling
- User session management
- Error responses for auth failures

### Error Handling

Reference web app patterns:
- Sentry integration (`@sentry/node`)
- Standard error response format
- User-friendly error messages
- Logging patterns from `/debug/` directory

## CLI Architecture

### File Structure

```
otu.ai.cli/
├── package.json           # Dependencies and metadata
├── otu.js                 # Main CLI application (single file architecture)
├── .env.template          # Environment variables template
├── .gitignore            # Git ignore patterns
└── supabase/             # Supabase configuration
    ├── config.toml       # Local Supabase config
    └── seed.sql          # Database seeds
```

### Key Components

1. **Authentication System**
   - FileStorage class for token persistence
   - OAuth flow via local Express server (port 3001)
   - Token storage in `~/.otu.authStorage.json`

2. **Command Processing**
   - Commander.js for CLI parsing
   - Interactive input handling
   - File reading capabilities

3. **Database Integration**
   - Direct Supabase client usage
   - Shared schema with web app
   - User-scoped queries

## Development Guidelines

### Code Style

When updating CLI, follow patterns from web app:
- Use TypeScript types where applicable
- Follow web app's error handling patterns
- Use consistent naming conventions
- Implement proper input validation

### Security

- Never commit sensitive credentials
- Use environment variables for all secrets
- Follow OAuth best practices from web app
- Implement proper token storage and cleanup

### Testing

While CLI currently lacks tests, reference web app's testing patterns:
- Jest framework (not Vitest)
- API integration tests
- Environment-specific test setup

## Common Development Tasks

### Adding New Commands

1. Add command to Commander.js setup in `otu.js`
2. Reference web app's API patterns for any Supabase operations
3. Follow existing command structure for consistency
4. Add proper error handling and user feedback

### Updating Database Integration

1. Check `otu.ai.web/supabase/migrations/` for schema changes
2. Update CLI queries to match new schema
3. Ensure backward compatibility where possible
4. Test with both local and production Supabase

### Updating Authentication

1. Reference `otu.ai.web/app/api/auth/` for current patterns
2. Update OAuth providers as needed
3. Ensure token storage remains secure
4. Test authentication flow end-to-end

## Integration with Web App

### Shared Resources

- **Supabase Instance**: Both CLI and web use same database
- **User System**: Shared authentication and user management
- **Data Schema**: CLI reads/writes to same tables as web app
- **API Standards**: Follow web app's response formats

### Complementary Usage

- CLI: Quick memo creation, scripting, automation
- Web App: Rich editing, AI features, advanced UI
- Users can create memos via CLI and enhance in web app

## Deployment

### CLI Distribution

The CLI is distributed as a Node.js package:
- Single file architecture for easy deployment
- Minimal dependencies for fast installation
- Cross-platform compatibility

### Version Management

- Version CLI separately from web app
- Document compatibility with web app versions
- Consider semantic versioning for breaking changes

## Important Notes

### Database Access

- Use same Supabase project as web app when possible
- Respect RLS policies from web app
- Use appropriate authentication context for user-scoped operations

### API Consistency

- Follow web app's error response format
- Use same field names and data types
- Implement similar rate limiting considerations

### Authentication Compatibility

- CLI should work with same user accounts as web app
- OAuth providers should match web app configuration
- Token handling should be secure and follow web app patterns