<p align="center">
  <img src="logo.png" width="200" alt="ucanet logo" />
</p>

<h1 align="center">ucanetlib</h1>
<p align="center">A Python library for domain/IP registry syncing over Git used by <a href="https://ucanet.net">ucanet.net</a>.</p>


---

## Table of Contents

- [Overview](#overview)
- [Requirements](#requirements)
- [Usage](#usage)
  - [Quick Start](#quick-start)
  - [Registering a Domain](#registering-a-domain)
  - [Registering an IP](#registering-an-ip)
  - [Looking Up Entries](#looking-up-entries)
  - [Configuration](#configuration)
- [Used By](#used-by)
- [License](#license)

---

## Overview

`ucanetlib` is a utility module for interacting with ucanet in Python that provides functionality for:

- Validating and formatting domains and IPs
- Managing user domain ownership and changes
- Auto-syncing changes to the GitHub domain registry
- Serving as the backend for all ucanet projects

---

## Requirements
To install requirements from CLI:

```bash
pip3 install -r requirements.txt
```

The following dependencies will be installed:
- tldextract
- gitpython
- apscheduler
- cachetools

---

## Usage

### Quick Start
Import it in your project:
```python
import ucanetlib
```

Initialize the repository syncing:
```python
ucanetlib.init_library()
```

This will:
- Pull the latest registry from Git
- Start auto-push/pull every 15s/10min
- Set up local cache

### Registering a domain
```python
ucanetlib.register_domain("example-domain.com", user_id=999999999999999999)
```
This reserves the domain for the Discorduser and assigns `0.0.0.0` by default.

### Registering an IP
```python
ucanetlib.register_ip("example-domain.com", user_id=999999999999999999, current_ip="1.2.3.4")
```
This sets or updates the IP assigned to the domain.

### Looking Up Entries
```python
ucanetlib.find_entry("example.com")
```
You can also see all domains owned by a user:
```python
ucanetlib.user_domains(user_id=999999999999999999)
```

### Configuration
You can customize `ucanetlib`'s behavior by setting module-level variables before calling `init_library`.
```python
import ucanetlib

ucanetlib.GIT_USERNAME = "your_username"
ucanetlib.GIT_PASSWORD = "your_token"
ucanetlib.GIT_URL = f"https://{ucanetlib.GIT_USERNAME}:{ucanetlib.GIT_PASSWORD}@github.com/your/repo.git"
ucanetlib.GIT_BRANCH = "main"
ucanetlib.REGISTRY_PATH = "./your-registry-folder/registry.txt"

ucanetlib.init_library()
```

Below are the available configuration variables:
| Variable       | Description                                                                  |
|----------------|------------------------------------------------------------------------------|
| `REGISTRY_PATH`| Path to the registry file (default: `./ucanet-registry/ucanet-registry.txt`) |
| `GIT_USERNAME` | Git username (used if changes will be published to the repo)                 |
| `GIT_PASSWORD` | Git token/password (used if changes will be published to the repo)           |
| `GIT_URL`      | Full HTTPS Git URL.                                                          |
| `GIT_BRANCH`   | Git branch to use (default: `"main"`)                                        |
| `GIT_PATH`     | Local path to clone the repo (default: `./ucanet-registry/`)                 |
| `CACHE_SIZE`   | Max number of cached domain entries (default: `3500`)                        |
| `CACHE_TTL`    | Time in seconds before cache expires (default: `600`)                        |

---

## Used By
- [ucanet-server](https://github.com/ucanet/ucanet-server)
- [ucanet-discord-bot](https://github.com/ucanet/ucanet-discord-bot)

---

## License
Licensed under the [AGPL-3.0 license](https://github.com/ucanet/ucanet-server#AGPL-3.0-1-ov-file).