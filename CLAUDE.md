# CLAUDE.md - AI Assistant Guide

This document provides comprehensive guidance for AI assistants working with the ArcaneChat/Chatmail relay server codebase.

## Table of Contents

- [Project Overview](#project-overview)
- [Repository Structure](#repository-structure)
- [Development Setup](#development-setup)
- [Architecture & Key Components](#architecture--key-components)
- [Development Workflows](#development-workflows)
- [Coding Conventions](#coding-conventions)
- [Testing Strategy](#testing-strategy)
- [Deployment Process](#deployment-process)
- [Common Tasks](#common-tasks)
- [Git Workflow](#git-workflow)
- [Important Considerations for AI Assistants](#important-considerations-for-ai-assistants)
- [Quick Reference](#quick-reference)

---

## Project Overview

**arcanechat.me** is a privacy-focused chatmail relay server based on the [chatmail project](https://github.com/deltachat/chatmail). It provides end-to-end encrypted email infrastructure optimized for instant messaging applications like Delta Chat.

### Core Principles

1. **Privacy First:** No tracking, minimal data retention, no personal information required
2. **Instant Onboarding:** Create-on-login semantics, no registration process
3. **Security:** OpenPGP encryption enforced, strict TLS, DKIM authentication
4. **Minimalism:** Lean infrastructure, battle-tested components (Postfix, Dovecot)
5. **Efficiency:** Messages stored only for transit, automatic cleanup

### Technology Stack

- **Backend:** Python 3.11+ (chatmaild services)
- **MTA:** Postfix (SMTP)
- **MDA:** Dovecot (IMAP)
- **Web Server:** Nginx
- **Certificates:** acmetool (Let's Encrypt)
- **Deployment:** pyinfra
- **Testing:** pytest
- **Web:** Vite + Bootstrap (ArcaneChat frontend)

---

## Repository Structure

```
/home/user/server/
├── chatmaild/              # Core Python services package
│   ├── src/chatmaild/      # Service implementations
│   │   ├── doveauth.py     # Authentication (create-on-login)
│   │   ├── filtermail.py   # Encryption enforcement
│   │   ├── metadata.py     # Push notifications & device tokens
│   │   ├── notifier.py     # Push notification delivery
│   │   ├── expire.py       # Message/user cleanup
│   │   ├── echo.py         # Test echobot
│   │   ├── metrics.py      # Prometheus metrics
│   │   ├── config.py       # Configuration management
│   │   └── tests/          # Unit tests
│   └── pyproject.toml      # Package configuration
│
├── cmdeploy/               # Deployment & management
│   ├── src/cmdeploy/
│   │   ├── cmdeploy.py     # CLI interface
│   │   ├── __init__.py     # Pyinfra deployment script
│   │   ├── deploy.py       # Deployment orchestration
│   │   ├── deploy_web.py   # Web deployment
│   │   ├── dns.py          # DNS validation
│   │   ├── www.py          # Web development server
│   │   ├── postfix/        # Postfix configuration templates
│   │   ├── dovecot/        # Dovecot configuration templates
│   │   ├── nginx/          # Nginx configuration templates
│   │   ├── service/        # Systemd service definitions
│   │   ├── opendkim/       # DKIM configuration
│   │   └── tests/          # Integration & online tests
│   └── pyproject.toml
│
├── www/                    # Web presence
│   ├── src/                # Default markdown-based site
│   │   ├── index.md        # Homepage
│   │   ├── info.md         # Information page
│   │   ├── privacy.md      # Privacy policy
│   │   └── page-layout.html # HTML template
│   └── arcanechat/         # Custom ArcaneChat web app
│       ├── index.html      # Main page
│       ├── privacy.html    # Privacy page
│       ├── src/            # JavaScript & SASS
│       ├── public/         # Static assets
│       └── package.json    # npm dependencies
│
├── scripts/                # Development convenience tools
│   ├── initenv.sh          # Bootstrap development environment
│   ├── cmdeploy            # cmdeploy wrapper script
│   └── dovecot/            # Dovecot build scripts
│
├── README.md               # Main documentation
├── ARCHITECTURE.md         # System architecture diagram
├── CHANGELOG.md            # Version history
└── chatmail.ini            # Main configuration (gitignored)
```

### Key Directories

- **chatmaild**: Python package with all core services (auth, filtering, notifications, cleanup)
- **cmdeploy**: Deployment automation, DNS management, testing, web development
- **www**: Web presence (both default markdown site and custom ArcaneChat app)
- **scripts**: Development bootstrapping and convenience wrappers

---

## Development Setup

### Initial Setup

```bash
# 1. Clone repository
git clone <repository-url>
cd server

# 2. Bootstrap development environment
scripts/initenv.sh

# This creates a Python virtualenv and installs:
# - chatmaild in editable mode
# - cmdeploy in editable mode
# - all dependencies

# 3. Create configuration file (for deployment)
scripts/cmdeploy init chat.example.org

# 4. Activate environment (if needed)
source venv/bin/activate
```

### Prerequisites

- **Python 3.11+** (required for crypt-r)
- **gcc** and **python3-dev** (for building dependencies)
- **SSH access** to target server (for deployment)
- **Node.js/pnpm** (for ArcaneChat web development)

### Directory-Specific Setup

#### Python Development (chatmaild/cmdeploy)
```bash
# Already handled by scripts/initenv.sh
# Both packages installed in editable mode
```

#### Web Development (ArcaneChat)
```bash
cd www/arcanechat
pnpm install
pnpm start          # Dev server on port 3000
```

#### Default Web Development
```bash
scripts/cmdeploy webdev
# Auto-opens browser, watches www/src/ for changes
```

---

## Architecture & Key Components

### Service Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    External Clients                      │
│            (Delta Chat, Email Clients, Web)              │
└─────────────────┬────────────────┬──────────────────────┘
                  │                │
         ┌────────▼────────┐  ┌────▼─────┐
         │  Postfix (SMTP) │  │  Dovecot │
         │  Port 25, 587   │  │  Port 993│
         └────────┬────────┘  └────┬─────┘
                  │                │
         ┌────────▼────────────────▼─────────┐
         │        chatmaild Services         │
         ├───────────────────────────────────┤
         │ • doveauth (authentication)       │
         │ • filtermail (encryption check)   │
         │ • metadata (push notifications)   │
         │ • notifier (notification delivery)│
         │ • expire (cleanup)                │
         └───────────────────────────────────┘
                  │
         ┌────────▼────────┐
         │  /home/vmail/   │
         │   (mailboxes)   │
         └─────────────────┘
```

### Core Components

#### 1. doveauth (Authentication Service)
- **File:** `chatmaild/src/chatmaild/doveauth.py`
- **Purpose:** Implements create-on-login semantics
- **How it works:**
  - Dovecot and Postfix query via Unix socket (`/run/doveauth/doveauth.socket`)
  - Uses Dovecot dict protocol
  - On first login: creates user mailbox, stores SHA512-CRYPT password
  - Subsequent logins: validates password against stored hash
  - Enforces username/password constraints (configurable in chatmail.ini)
- **Important:** The echobot account (`echo@domain`) is never created in the database

#### 2. filtermail (Encryption Enforcement)
- **File:** `chatmaild/src/chatmaild/filtermail.py`
- **Purpose:** Ensures only OpenPGP-encrypted messages are relayed
- **How it works:**
  - Acts as SMTP proxy (intercepts mail flow)
  - Two instances: outgoing (port 10025) and incoming (port 10081)
  - Validates message has OpenPGP armor (BEGIN PGP MESSAGE)
  - Checks MIME structure compliance
  - Implements rate limiting (60 msgs/min default)
  - Allows exceptions for securejoin protocol
  - CPU-intensive operations run in thread pool
- **Exceptions:** Users can opt-out of incoming encryption enforcement by removing `enforceE2EEincoming` file

#### 3. chatmail-metadata (Device Metadata)
- **File:** `chatmaild/src/chatmaild/metadata.py`
- **Purpose:** Manages device tokens and configuration
- **How it works:**
  - Dovecot Lua script calls via Unix socket
  - Stores push notification tokens (90-day expiry)
  - Provides TURN credentials
  - Manages Iroh relay configuration
  - Triggers push notifications on new mail
- **Storage:** Filesystem-based (filedict) in user's mailbox

#### 4. notifier (Push Notification Delivery)
- **File:** `chatmaild/src/chatmaild/notifier.py`
- **Purpose:** Delivers push notifications to Apple/Google/Huawei
- **How it works:**
  - Multi-threaded dispatcher (ThreadPoolExecutor)
  - Persistent queue (filesystem-based)
  - Exponential backoff retry (base: 8s)
  - Posts to `notifications.delta.chat`
  - Drops notifications after 5 hours
- **Note:** Network failures are handled gracefully

#### 5. chatmail-expire (Cleanup Service)
- **File:** `chatmaild/src/chatmaild/expire.py`
- **Purpose:** Automatic message and user deletion
- **How it works:**
  - Runs daily via systemd timer
  - Deletes messages older than 20 days (configurable)
  - Deletes large messages (>200KB) after 7 days
  - Removes inactive users after 90 days
  - Supports dry-run mode
- **Configuration:** `delete_mails_after`, `delete_inactive_users_after` in chatmail.ini

### Configuration Management

All configuration is driven by **`chatmail.ini`** (created by `cmdeploy init`):

```ini
[chatmail]
mail_domain = chat.example.org
privacy_mail = privacy@example.org
privacy_supervisor = Supervisor Name

[auth]
username_min_length = 3
username_max_length = 15
password_min_length = 9

[mail]
delete_mails_after = 20d
delete_inactive_users_after = 90d
```

Configuration is rendered into service configs via Jinja2 templates in `cmdeploy/src/cmdeploy/`.

---

## Development Workflows

### Working with chatmaild Services

#### Running Services Locally

```bash
# Activate virtualenv (if not already active)
source venv/bin/activate

# Run individual services
doveauth --config chatmail.ini
filtermail --config chatmail.ini
chatmail-metadata --config chatmail.ini

# Run with debug logging
PYTHONPATH=chatmaild/src python -m chatmaild.doveauth --config chatmail.ini
```

#### Testing Changes

```bash
# Run chatmaild unit tests
cd chatmaild
pytest

# Run specific test file
pytest src/chatmaild/tests/test_doveauth.py

# Run with verbose output
pytest -v

# Run with coverage
pytest --cov=chatmaild
```

### Working with cmdeploy

#### Common Commands

```bash
scripts/cmdeploy init chat.example.org    # Create chatmail.ini
scripts/cmdeploy dns                      # Check DNS records
scripts/cmdeploy run                      # Deploy to server
scripts/cmdeploy status                   # Check service status
scripts/cmdeploy test                     # Run online tests
scripts/cmdeploy bench                    # Performance benchmark
scripts/cmdeploy web                      # Deploy web only
scripts/cmdeploy webdev                   # Local web dev server
scripts/cmdeploy fmt                      # Format code with ruff
```

#### Deployment Options

```bash
# Dry run (show changes without applying)
scripts/cmdeploy run --dry-run

# Skip DNS validation
scripts/cmdeploy run --skip-dns-check

# Disable mail services (useful for migration)
scripts/cmdeploy run --disable-mail

# Deploy to specific host
scripts/cmdeploy run --ssh-host 1.2.3.4

# Local deployment (testing)
scripts/cmdeploy run --ssh-host @local
```

### Web Development

#### ArcaneChat Site

```bash
cd www/arcanechat

# Install dependencies
pnpm install

# Development server (port 3000)
pnpm start

# Production build
pnpm build

# Auto-format with Prettier
pnpm fix

# Check formatting
pnpm check
```

#### Default Markdown Site

```bash
# Start development server
scripts/cmdeploy webdev

# This:
# - Builds HTML from www/src/*.md
# - Watches for changes
# - Auto-refreshes browser
# - Outputs to www/build/
```

### Debugging

#### View Service Logs (on server)

```bash
# SSH to server
ssh root@chat.example.org

# View service logs
journalctl -u doveauth -f
journalctl -u filtermail -f
journalctl -u chatmail-metadata -f
journalctl -u postfix -f
journalctl -u dovecot -f

# View all chatmail services
journalctl -u "chatmail-*" -f

# Check service status
systemctl status doveauth
systemctl status filtermail
```

#### Emergency Commands

```bash
# Disable account creation (on server)
touch /etc/chatmail-nocreate

# Re-enable account creation
rm /etc/chatmail-nocreate

# Restart all chatmail services
systemctl restart doveauth chatmail-metadata filtermail filtermail-incoming

# Reload Postfix/Dovecot after config changes
systemctl reload postfix
systemctl reload dovecot
```

---

## Coding Conventions

### Python Style

This project follows **PEP 8** enforced by **ruff**.

#### General Guidelines

1. **Use type hints** for function signatures (but not extensively throughout)
2. **Clear, descriptive names** for functions and variables
3. **Comprehensive docstrings** for complex functions
4. **Inline comments** for non-obvious logic
5. **Keep functions focused** on a single responsibility

#### Example from doveauth.py

```python
def encrypt_password(password: str):
    """Encrypt password using SHA512-CRYPT for Dovecot."""
    # https://doc.dovecot.org/configuration_manual/authentication/password_schemes/
    passhash = crypt_r.crypt(password, crypt_r.METHOD_SHA512)
    return "{SHA512-CRYPT}" + passhash


def is_allowed_to_create(config: Config, user, cleartext_password) -> bool:
    """Return True if user and password are admissible."""
    if os.path.exists(NOCREATE_FILE):
        logging.warning(f"blocked account creation because {NOCREATE_FILE!r} exists.")
        return False

    if len(cleartext_password) < config.password_min_length:
        logging.warning(
            "Password needs to be at least %s characters long",
            config.password_min_length,
        )
        return False
    # ... more validation
```

#### Import Organization

Imports are organized by **ruff/isort**:

```python
# 1. Standard library
import json
import logging
import os
import sys

# 2. Third-party
try:
    import crypt_r
except ImportError:
    import crypt as crypt_r

# 3. Local
from .config import Config, read_config
from .dictproxy import DictProxy
```

#### Logging

Use Python's logging module:

```python
import logging

# Warning level for expected errors
logging.warning(f"user {user!r} is not a proper e-mail address")

# Info level for normal operations
logging.info(f"Created new user: {user}")

# Error level for unexpected failures
logging.error(f"Failed to process message: {exc}")
```

### Configuration via pyproject.toml

Both packages use **ruff** for linting:

```toml
[tool.ruff]
lint.select = [
  "F",    # Pyflakes
  "I",    # isort
  "PLC",  # Pylint Convention
  "PLE",  # Pylint Error
  "PLW",  # Pylint Warning
]
lint.ignore = [
  "PLC0415"  # import-outside-top-level
]
```

#### Running Linters

```bash
# Format code
ruff format chatmaild/src/
ruff format cmdeploy/src/

# Check for issues
ruff check chatmaild/src/
ruff check cmdeploy/src/

# Auto-fix issues
ruff check --fix chatmaild/src/

# Or use cmdeploy wrapper
scripts/cmdeploy fmt
```

### Template Conventions (Jinja2)

Configuration templates use `.j2` extension:

```jinja2
# Example from postfix/main.cf.j2
myhostname = {{ mail_domain }}
myorigin = $myhostname

# Conditional sections
{% if disable_ipv6 %}
inet_protocols = ipv4
{% else %}
inet_protocols = all
{% endif %}
```

### File Naming Conventions

- **Python modules:** `lowercase_with_underscores.py`
- **Jinja2 templates:** `config_name.j2`
- **Systemd services:** `service-name.service.f` (`.f` = formatted/template)
- **Markdown:** `UPPERCASE.md` for docs, `lowercase.md` for web content
- **Tests:** `test_module_name.py`

---

## Testing Strategy

### Test Layers

#### 1. Unit Tests (chatmaild)

**Location:** `chatmaild/src/chatmaild/tests/`

**Coverage:**
- Authentication (`test_doveauth.py`)
- Mail filtering (`test_filtermail.py`, `test_filtermail_blackbox.py`)
- Metadata handling (`test_metadata.py`)
- Expiry logic (`test_expire.py`, `test_delete_inactive_users.py`)
- Configuration (`test_config.py`)

**Running:**
```bash
cd chatmaild
pytest                          # All tests
pytest -v                       # Verbose
pytest tests/test_doveauth.py   # Specific test
pytest -k "test_encrypt"        # Match pattern
```

**Fixtures:** Defined in `chatmaild/src/chatmaild/tests/plugin.py`
- `make_config` - Test configuration factory
- `gencreds` - Credential generator
- `maildata` - Test email data loader
- `mockout` - Output capture

#### 2. Integration Tests (cmdeploy offline)

**Location:** `cmdeploy/src/cmdeploy/tests/`

**Coverage:**
- DNS validation (`test_dns.py`)
- CLI commands (`test_cmdeploy.py`)
- Helper functions (`test_helpers.py`)

**Running:**
```bash
cd cmdeploy
pytest                     # All offline tests
```

#### 3. Online Tests (cmdeploy)

**Location:** `cmdeploy/src/cmdeploy/tests/online/`

**Requirements:** Live chatmail server configured in `chatmail.ini`

**Coverage:**
- Account creation and authentication (`test_0_login.py`)
- QR code functionality (`test_0_qr.py`)
- Basic email operations (`test_1_basic.py`)
- Delta Chat integration (`test_2_deltachat.py`)
- Performance benchmarks (`benchmark.py`)

**Running:**
```bash
scripts/cmdeploy test           # All tests
scripts/cmdeploy test --slow    # Include slow tests
scripts/cmdeploy bench          # Benchmarks only
```

**Fixtures:** Defined in `cmdeploy/src/cmdeploy/tests/plugin.py`
- `chatmail_config` - Reads chatmail.ini
- `imap`, `smtp` - Protocol connection helpers
- `gencreds` - Credential generation
- `cmfactory` - Delta Chat account factory
- `benchmark` - Performance measurement

### Test-Driven Development

When adding new features:

1. **Write unit tests first** (in chatmaild/tests/)
2. **Implement the feature** (in chatmaild/src/)
3. **Add integration tests** if needed (cmdeploy/tests/)
4. **Test deployment** with `cmdeploy run --dry-run`
5. **Run online tests** with `cmdeploy test`

### Mocking & Test Data

Test email data is stored in `chatmaild/src/chatmaild/tests/mail-data/`:

```python
# Example from test fixture
def maildata(tmpdir):
    """Load test email data."""
    mail_data_dir = Path(__file__).parent / "mail-data"
    return mail_data_dir
```

---

## Deployment Process

### Overview

Deployment uses **pyinfra** to automate server configuration. The process is idempotent (safe to re-run).

### Deployment Pipeline

```
┌─────────────────────────────────────────────────────────┐
│ 1. Local Build Phase                                    │
│    • Build chatmaild package (.tar.gz)                  │
│    • Read chatmail.ini configuration                    │
└─────────────────┬───────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────┐
│ 2. Remote Deployment Phase                              │
│    • SSH to root@<mail_domain>                          │
│    • Upload chatmaild package                           │
│    • Create virtualenv at /usr/local/lib/chatmaild/     │
│    • Install dependencies (gcc, python3-dev, etc.)      │
│    • Install chatmaild in virtualenv                    │
└─────────────────┬───────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────┐
│ 3. Service Configuration Phase                          │
│    • Install system packages (Postfix, Dovecot, etc.)   │
│    • Render Jinja2 templates                            │
│    • Upload configs to system locations                 │
│    • Create system users (vmail, echobot)               │
│    • Install systemd services                           │
└─────────────────┬───────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────┐
│ 4. Certificate & Web Phase                              │
│    • Configure acmetool (Let's Encrypt)                 │
│    • Request/renew certificates                         │
│    • Build and deploy website                           │
│    • Enable and start all services                      │
└─────────────────────────────────────────────────────────┘
```

### Key Files

- **`cmdeploy/src/cmdeploy/__init__.py`** - Main pyinfra deployment script
- **`cmdeploy/src/cmdeploy/deploy.py`** - Deployment orchestration
- **`cmdeploy/src/cmdeploy/deploy_web.py`** - Web deployment
- **`cmdeploy/src/cmdeploy/cmdeploy.py`** - CLI interface

### Deployment Targets

1. **Production:** `scripts/cmdeploy run` (uses mail_domain from chatmail.ini)
2. **Specific Host:** `scripts/cmdeploy run --ssh-host 1.2.3.4`
3. **Local:** `scripts/cmdeploy run --ssh-host @local`
4. **Docker:** `scripts/cmdeploy run --ssh-host @docker`

### Important Paths on Server

```
/usr/local/lib/chatmaild/          # Virtualenv & package
├── venv/                          # Python virtualenv
├── dist/                          # Uploaded chatmaild package
└── chatmail.ini                   # Configuration

/home/vmail/mail/                  # User mailboxes
├── user@domain/                   # Per-user directory
│   ├── password                   # SHA512-CRYPT hash
│   ├── enforceE2EEincoming        # Encryption flag
│   ├── cur/, new/, tmp/           # Maildir folders
│   └── metadata.json              # Device tokens

/etc/postfix/                      # Postfix config
/etc/dovecot/                      # Dovecot config
/etc/nginx/                        # Nginx config
/etc/systemd/system/               # Systemd services
/var/lib/acme/                     # TLS certificates
/etc/dkimkeys/                     # DKIM keys
/var/www/html/                     # Website
```

### DNS Requirements

Before deploying, configure these DNS records (checked by `cmdeploy dns`):

```
chat.example.org.           A       1.2.3.4
chat.example.org.           AAAA    2001:db8::1
www.chat.example.org.       CNAME   chat.example.org.
mta-sts.chat.example.org.   CNAME   chat.example.org.
chat.example.org.           MX 10   chat.example.org.
chat.example.org.           TXT     "v=spf1 mx -all"
default._domainkey.chat...  TXT     "v=DKIM1; k=rsa; p=..."
_dmarc.chat.example.org.    TXT     "v=DMARC1; p=reject; ..."
_mta-sts.chat.example.org.  TXT     "v=STSv1; id=..."
```

### Deployment Best Practices

1. **Always test with `--dry-run` first**
2. **Check DNS before deploying:** `scripts/cmdeploy dns`
3. **Monitor logs during deployment:** `journalctl -f`
4. **Run tests after deployment:** `scripts/cmdeploy test`
5. **Keep chatmail.ini in version control** (in a private repo)

---

## Common Tasks

### Adding a New Service

1. **Create service module** in `chatmaild/src/chatmaild/newservice.py`
2. **Add entry point** to `chatmaild/pyproject.toml`:
   ```toml
   [project.scripts]
   newservice = "chatmaild.newservice:main"
   ```
3. **Create systemd unit** in `cmdeploy/src/cmdeploy/service/newservice.service.f`
4. **Update deployment script** in `cmdeploy/src/cmdeploy/__init__.py`:
   ```python
   systemd.service(
       name="newservice",
       service_content=read_template("service/newservice.service.f"),
       running=True,
       enabled=True,
   )
   ```
5. **Write tests** in `chatmaild/src/chatmaild/tests/test_newservice.py`
6. **Test locally:** `pytest chatmaild/tests/test_newservice.py`
7. **Deploy:** `scripts/cmdeploy run --dry-run` then `scripts/cmdeploy run`

### Modifying Configuration Templates

1. **Edit template** in `cmdeploy/src/cmdeploy/<service>/<config>.j2`
2. **Test rendering** by running `scripts/cmdeploy run --dry-run`
3. **Deploy:** `scripts/cmdeploy run`
4. **Verify on server:** `cat /etc/<service>/<config>`
5. **Check service:** `systemctl status <service>`

### Adding Configuration Options

1. **Add to template** in `chatmaild/src/chatmaild/ini/chatmail.ini.f`:
   ```ini
   [section]
   new_option = default_value
   ```
2. **Add to Config class** in `chatmaild/src/chatmaild/config.py`:
   ```python
   @property
   def new_option(self):
       return self._iniconfig.get("section", "new_option", fallback="default_value")
   ```
3. **Use in service:**
   ```python
   config = read_config()
   value = config.new_option
   ```
4. **Document in README.md** or code comments

### Updating Dependencies

#### Python Dependencies

**chatmaild:**
```bash
# Edit chatmaild/pyproject.toml
[project]
dependencies = [
  "new-package>=1.0",
]

# Reinstall
pip install -e chatmaild/
```

**cmdeploy:**
```bash
# Edit cmdeploy/pyproject.toml
[project]
dependencies = [
  "new-package>=1.0",
]

# Reinstall
pip install -e cmdeploy/
```

#### Web Dependencies (ArcaneChat)

```bash
cd www/arcanechat
pnpm add new-package
pnpm add -D new-dev-package
```

### Running Code Formatting

```bash
# Format all Python code
ruff format chatmaild/src/
ruff format cmdeploy/src/

# Or use wrapper
scripts/cmdeploy fmt

# Check without modifying
ruff format --check chatmaild/src/
```

### Updating the Web Page

#### ArcaneChat Site

```bash
cd www/arcanechat

# Edit files
vim index.html
vim src/style.scss
vim src/main.js

# Test locally
pnpm start

# Build for production
pnpm build

# Deploy
scripts/cmdeploy web
```

#### Default Markdown Site

```bash
# Edit markdown files
vim www/src/index.md
vim www/src/privacy.md

# Test locally
scripts/cmdeploy webdev

# Deploy
scripts/cmdeploy web
```

### Checking Server Status

```bash
# Overall status
scripts/cmdeploy status

# Specific service (on server)
ssh root@chat.example.org
systemctl status doveauth
systemctl status filtermail
systemctl status postfix
systemctl status dovecot

# View logs
journalctl -u doveauth -n 100
journalctl -u filtermail -f

# Check metrics
curl https://chat.example.org/metrics
```

---

## Git Workflow

### Branching Strategy

- **Main branch:** `main` (or unspecified)
- **Feature branches:** `claude/<session-id>` (for Claude Code sessions)
- **Upstream:** Syncs from `deltachat/chatmail` (chatmail project)

### Commit Message Conventions

Follow these patterns based on existing commits:

```bash
# Format: lowercase, imperative mood, concise
update index.html
fix web_cmd
update social links and avoid header and footer duplication
Require STARTTLS for incoming port 25 connections
filtermail: improve check_armored_payload()
nginx: be more specific with the server name
```

**Guidelines:**
- Use **lowercase** (except for proper nouns)
- Use **imperative mood** ("add" not "added", "fix" not "fixed")
- Be **concise** but descriptive
- **Prefix with module** for module-specific changes (e.g., `filtermail:`, `nginx:`)
- Keep **under 72 characters** for the first line
- No period at the end

### Workflow for Changes

```bash
# 1. Ensure you're on the right branch
git status
git branch

# 2. Make changes
# ... edit files ...

# 3. Review changes
git status
git diff

# 4. Stage changes
git add path/to/file

# 5. Commit with clear message
git commit -m "update authentication to support new protocol"

# 6. Push to branch
git push -u origin claude/<session-id>

# Note: Branch must start with 'claude/' and match session ID
```

### Important Git Practices

1. **Never push to main/master directly**
2. **Always push to the designated feature branch**
3. **Use descriptive commit messages**
4. **Commit frequently** (but ensure each commit is logical)
5. **Don't commit sensitive data** (chatmail.ini, credentials, etc.)

### Files to Never Commit

These are already in `.gitignore`:

```
chatmail.ini          # Server configuration
*.pyc                 # Python bytecode
__pycache__/          # Python cache
venv/                 # Virtual environment
.env                  # Environment variables
node_modules/         # npm packages
dist/                 # Build artifacts
*.egg-info/           # Python package metadata
```

### Syncing from Upstream

This repository forks from `deltachat/chatmail`:

```bash
# Add upstream remote (one time)
git remote add upstream https://github.com/deltachat/chatmail.git

# Sync from upstream
git fetch upstream
git merge upstream/main

# Resolve conflicts if any
# ... edit conflicted files ...
git add .
git commit -m "Merge remote-tracking branch 'upstream/main'"
```

---

## Important Considerations for AI Assistants

### Key Principles

1. **Read before modifying:** Always read files before suggesting changes
2. **Understand context:** Review related files and dependencies
3. **Test thoroughly:** Run tests before committing
4. **Follow conventions:** Match existing code style and patterns
5. **Document changes:** Update comments, docstrings, and this CLAUDE.md as needed
6. **Be cautious with security:** This is email infrastructure—security is critical

### Common Pitfalls to Avoid

#### 1. Authentication & Security

**NEVER:**
- Weaken password requirements
- Bypass encryption checks
- Disable DKIM validation
- Allow unauthenticated access
- Log passwords or sensitive data

**ALWAYS:**
- Maintain SHA512-CRYPT for passwords
- Enforce OpenPGP encryption (unless explicitly opt-out)
- Validate DKIM signatures
- Use secure defaults
- Follow principle of least privilege

#### 2. Configuration Changes

**NEVER:**
- Hardcode values that should be in chatmail.ini
- Change defaults without understanding impact
- Bypass configuration validation
- Ignore DNS requirements

**ALWAYS:**
- Use Config class for all configuration
- Provide sensible defaults
- Validate configuration values
- Document new options

#### 3. Service Communication

**NEVER:**
- Change Unix socket paths without updating all references
- Modify dict protocol format without testing
- Break backward compatibility with Dovecot/Postfix
- Ignore communication errors

**ALWAYS:**
- Use established communication patterns
- Handle errors gracefully
- Log communication issues
- Test integration thoroughly

#### 4. Data Storage

**NEVER:**
- Modify maildir structure directly
- Delete user data without confirmation
- Store sensitive data in logs
- Use SQL databases (filesystem-based storage is intentional)

**ALWAYS:**
- Use established User/Config abstractions
- Preserve maildir format compatibility
- Respect user privacy
- Use filedict for metadata

### Testing Requirements

Before submitting changes:

1. **Run unit tests:** `pytest chatmaild/`
2. **Run linter:** `ruff check chatmaild/src/`
3. **Format code:** `ruff format chatmaild/src/`
4. **Test deployment:** `scripts/cmdeploy run --dry-run`
5. **Run online tests:** `scripts/cmdeploy test` (if possible)

### When to Ask for Clarification

- Changing authentication logic
- Modifying encryption enforcement
- Altering DNS requirements
- Changing service communication protocols
- Modifying Postfix/Dovecot integration
- Adjusting security-related timeouts or limits

### Understanding the Ecosystem

This chatmail relay integrates with:

- **Delta Chat:** Primary client application
- **Standard email clients:** Via IMAP/SMTP
- **Push notification service:** notifications.delta.chat
- **Iroh relay:** P2P networking
- **TURN server:** WebRTC calling
- **Let's Encrypt:** Certificate authority
- **DNS providers:** Various (configured by users)

Changes must maintain compatibility with all these systems.

### Performance Considerations

- **filtermail** runs CPU-intensive operations in thread pool
- **notifier** uses multi-threading for HTTP requests
- **doveauth** must respond quickly (blocks login)
- **metadata** is called on every new message
- Keep service startup time minimal

### Privacy & Legal

This is privacy-focused infrastructure:

- **No tracking or analytics** in core services
- **Minimal data retention** (messages deleted after 20 days)
- **No personal information collection**
- **GDPR compliance** by design
- Respect these principles in all changes

---

## Quick Reference

### File Locations Cheat Sheet

```bash
# Configuration
chatmail.ini                           # Main config (gitignored)
chatmaild/src/chatmaild/config.py      # Config parser
cmdeploy/src/cmdeploy/cmdeploy.py      # CLI commands

# Core Services
chatmaild/src/chatmaild/doveauth.py    # Authentication
chatmaild/src/chatmaild/filtermail.py  # Encryption enforcement
chatmaild/src/chatmaild/metadata.py    # Push notifications
chatmaild/src/chatmaild/expire.py      # Cleanup

# Templates
cmdeploy/src/cmdeploy/postfix/         # Postfix configs
cmdeploy/src/cmdeploy/dovecot/         # Dovecot configs
cmdeploy/src/cmdeploy/nginx/           # Nginx configs
cmdeploy/src/cmdeploy/service/         # Systemd services

# Tests
chatmaild/src/chatmaild/tests/         # Unit tests
cmdeploy/src/cmdeploy/tests/           # Integration tests
cmdeploy/src/cmdeploy/tests/online/    # Online tests

# Web
www/src/                               # Default markdown site
www/arcanechat/                        # Custom web app
```

### Command Reference

```bash
# Setup
scripts/initenv.sh                     # Bootstrap environment
scripts/cmdeploy init <domain>         # Create config

# Development
scripts/cmdeploy webdev                # Web dev server
scripts/cmdeploy fmt                   # Format code
pytest chatmaild/                      # Run unit tests

# Deployment
scripts/cmdeploy dns                   # Check DNS
scripts/cmdeploy run --dry-run         # Test deployment
scripts/cmdeploy run                   # Deploy
scripts/cmdeploy status                # Check status
scripts/cmdeploy test                  # Run online tests

# Debugging (on server)
journalctl -u doveauth -f              # View service logs
systemctl status filtermail            # Check service
systemctl restart chatmail-metadata    # Restart service
```

### Environment Variables

```bash
# Python path (for local development)
export PYTHONPATH=chatmaild/src:cmdeploy/src

# Activate virtualenv
source venv/bin/activate
```

### Port Reference

```
25    SMTP (incoming mail)
80    HTTP (acmetool, redirects to 443)
143   IMAP (legacy, use 993)
443   HTTPS (multiplexed: web, IMAP, SMTP via ALPN)
465   SUBMISSIONS (SMTP over TLS)
587   SUBMISSION (SMTP with STARTTLS)
993   IMAPS (IMAP over TLS)
3478  STUN/TURN (UDP, chatmail-turn)
8443  HTTPS-ALT (Nginx internal)
10025 filtermail outgoing (internal)
10081 filtermail incoming (internal)
```

### Log Levels

```python
logging.debug()    # Detailed diagnostic info
logging.info()     # Normal operations
logging.warning()  # Expected errors (bad password, etc.)
logging.error()    # Unexpected failures
logging.critical() # System failures
```

---

## Additional Resources

- **Main README:** [README.md](README.md)
- **Architecture Diagram:** [ARCHITECTURE.md](ARCHITECTURE.md)
- **Changelog:** [CHANGELOG.md](CHANGELOG.md)
- **Upstream Project:** https://github.com/deltachat/chatmail
- **Delta Chat:** https://delta.chat
- **pyinfra Docs:** https://pyinfra.com
- **Dovecot Docs:** https://doc.dovecot.org
- **Postfix Docs:** https://www.postfix.org/documentation.html

---

## Updating This Document

When making significant changes to the codebase:

1. **Update relevant sections** in this CLAUDE.md
2. **Add new sections** if introducing new concepts
3. **Keep examples current** with actual code
4. **Update command references** if CLI changes
5. **Maintain consistency** with README.md and ARCHITECTURE.md

This document should always reflect the current state of the codebase and serve as a reliable guide for AI assistants.

---

**Document Version:** 1.0
**Last Updated:** 2025-11-24
**Codebase Version:** chatmaild 0.2, cmdeploy 0.2
