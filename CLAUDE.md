# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Ansible collection (`hitachivantara.raidcom`) for managing Hitachi VSP storage systems via Hitachi CCI/RAIDCOM CLI. This is a community-supported collection for a specific use case — Hitachi Vantara's official collection is at `hitachi-vantara/vspone-block-ansible`.

## Build and Deploy

```bash
# Build the collection tarball
ansible-galaxy collection build --force

# Install locally (version must match galaxy.yml)
ansible-galaxy collection install hitachivantara-raidcom-0.6.2.tar.gz --force
```

After ANY change to `plugins/module_utils/` or `plugins/modules/`, always rebuild and reinstall.

## CI/CD

GitHub Actions workflow (`.github/workflows/publish.yml`) triggers on push to `main`:
- Builds a Docker image with Ansible, pushes to GHCR
- Builds the collection tarball inside the container
- Publishes to Ansible Galaxy using `ANSIBLE_COLLECTIONS_TOKEN` secret

## Architecture

This is a standard Ansible collection with namespace `hitachivantara.raidcom`.

### Key Components

- **`plugins/modules/hur.py`** — The HUR (Hitachi Universal Replication) module. Handles async replication pair management (create, split, resync, takeover, delete, query, chkdsp). States map to CCI pair operations.
- **`plugins/module_utils/hitachi_raidcom.py`** — Shared utility class `hitachi_raidcom` wrapping the `hiraid` Python library. Provides volume management, host group management, and HUR operations. Logs to `/var/log/hitachi_raidcom_collection.log`.

### External Dependency: hiraid

The `hiraid` library (installed from `requirements.yml` pointing to `github.com/hv-ps/python.package`) provides the actual RAIDCOM/CCI command execution via `hiraid.raidcom.Raidcom` and `hiraid.horcm.horcm_cci.Cci` classes. All storage CLI interactions go through hiraid — this collection is the Ansible interface layer.

### Module Pattern

Modules define an `argument_spec` that merges module-specific params with `hitachi_raidcom_argument_spec` (common params: `storage_serial`, `horcm_inst`). The module instantiates `hitachi_raidcom(module)` and dispatches based on the `state` parameter.

## Prerequisites

- Python >= 3.6.8, Ansible >= 2.11.0
- CCI >= 1-66-03/01 installed and configured (HORCM files, horcmstart, login)
- `hiraid` Python library >= 1.0.16

## Tests

Test playbooks are in `tests/` (e.g., `hur_create.yml`, `hur_split_1.yml`). These are Ansible playbooks that require a live Hitachi storage environment with CCI configured — they are not unit tests.

## Collection Version

Version is tracked in `galaxy.yml` (currently `0.6.2`). The CI publish workflow also references the version — keep both in sync.
