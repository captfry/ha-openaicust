# Repository Guidelines

## Project Structure & Module Organization

This repository contains a Home Assistant custom integration. Runtime code lives in `custom_components/openai_api_conversation/`.

- `manifest.json` declares the domain, dependencies, version, and Python requirement.
- `config_flow.py` handles setup and options flows.
- `conversation.py` contains the agent behavior and OpenAI API interaction.
- `const.py` centralizes option keys and recommended defaults.
- `services.yaml`, `strings.json`, `icons.json`, and `translations/en.json` define service metadata and UI text.
- `.devcontainer/` builds a Home Assistant Core dev environment and links the component into HA config.
- `.github/workflows/hassfest.yaml` runs hassfest validation in CI.

There is currently no dedicated `tests/` directory in this repository.

## Build, Test, and Development Commands

- `hass`: starts Home Assistant in the devcontainer using `/workspaces/core/config`.
- `pytest tests/`: runs tests from the devcontainer when relevant tests are added or mounted.
- `python -m compileall custom_components/openai_api_conversation`: quick syntax check for the integration code.
- `hassfest`: validates integration metadata if available locally; CI runs `home-assistant/actions/hassfest@master`.

For manual testing, use the devcontainer. Its setup script installs Home Assistant Core and symlinks this integration into `config/custom_components/openai_api_conversation`.

## Coding Style & Naming Conventions

Use Python 3 style with four-space indentation, type annotations for public helpers and async callbacks, and Home Assistant async names such as `async_step_user`. Keep constants in `const.py` using uppercase names, for example `CONF_BASE_URL` and `RECOMMENDED_CHAT_MODEL`.

Group imports as standard library, third-party packages, Home Assistant imports, then local imports. The devcontainer uses Ruff formatting and Pylance basic type checking.

## Testing Guidelines

Prefer focused tests for config flow behavior, option schema changes, and OpenAI client interactions. Name tests after behavior, for example `test_config_flow_invalid_auth`. Mock OpenAI calls; do not require real API keys or network access.

Before opening a pull request, run a syntax check and validate the integration in Home Assistant. Ensure metadata changes pass hassfest.

## Commit & Pull Request Guidelines

Recent commits use short, imperative or descriptive lowercase messages, for example `add missing issue tracker`. Keep commits focused on one behavior or metadata change.

Pull requests should include a concise description, testing performed, and any Home Assistant UI impact. Link related issues when applicable. Include screenshots for visible config flow, options flow, service, or translation changes.

## Security & Configuration Tips

Never commit API keys, Home Assistant secrets, or local configuration files with credentials. Use the placeholder API key behavior in `const.py` for examples. Treat custom base URLs as user configuration and avoid logging sensitive values.
