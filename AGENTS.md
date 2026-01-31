# Agent Guide for Debuginn Blog

This repository hosts the static blog for [debuginn.com](https://blog.debuginn.com), built with **Hugo** and the **Stack** theme. It uses Go modules to manage the theme dependency.

## 🛠 Project Setup & Environment

### Prerequisites
- **Hugo Extended**: Latest version required (checked in CI).
- **Go**: Version 1.17+ (required for Hugo modules).
- **Git**: With submodule support.

### Installation
Clone with submodules:
```bash
git clone --recursive https://github.com/debuginn/blog.git
```

## 🚀 Build & Development Commands

### Local Development
Start the development server with live reload:
```bash
hugo server
```
Access at `http://localhost:1313`.

### Production Build
Build the static site (output to `public/`):
```bash
hugo --minify --gc
```
- `--minify`: Minifies HTML, CSS, JS, XML, JSON.
- `--gc`: Runs garbage collector to remove unused cache files.

### Dependency Management (Theme)
Update the Hugo theme module:
```bash
hugo mod get -u github.com/CaiJimmy/hugo-theme-stack/v3
hugo mod tidy
```

## 📂 Directory Structure

- **`content/`**: Markdown content files (posts, pages). Managed as a git submodule.
- **`config/`**: Configuration files (split by purpose).
  - `_default/config.toml`: Main site configuration.
  - `_default/menu.toml`: Navigation menu definitions.
  - `_default/params.toml`: Theme-specific parameters.
- **`layouts/`**: HTML templates (overrides theme defaults).
- **`static/`**: Static assets (images, files) copied directly to root.
- **`assets/`**: Assets processed by Hugo Pipes (SCSS, JS).
- **`public/`**: Generated static site (ignored in git, deployment artifact).

## 📝 Code Style & Conventions

### Content (Markdown)
- **Frontmatter**: Use TOML or YAML (consistent with existing files).
- **Images**: Store in `static/` or page bundles. Use standard Markdown image syntax or Hugo shortcodes.
- **Headings**: Use ATX style (`# Heading`) instead of Setext style.
- **Language**: Default language is Chinese (`zh`).

### Templates (HTML/Go Templates)
- **Syntax**: Standard Go template syntax `{{ ... }}`.
- **Indentation**: Use 2 or 4 spaces consistently.
- **Partials**: Reuse code via `{{ partial "name" . }}`.
- **Variables**: Use meaningful variable names. Avoid complex logic in templates; move to partials or Hugo pipes if possible.

### Configuration (TOML)
- Keep configuration split in `config/_default/`.
- Use comments to explain non-obvious settings.
- Group related settings (e.g., all social media links together).

## 🔄 CI/CD Workflow

The project uses GitHub Actions (`.github/workflows/deploy.yml`) for deployment:
1.  **Trigger**: Push to `main`, `workflow_dispatch`, or `repository_dispatch` (content-updated).
2.  **Submodules**: Updates `content` submodule to latest.
3.  **Check**: Verifies if content actually changed to avoid redundant builds.
4.  **Build**: Sets up Go & Hugo, caches resources, runs `hugo --minify --gc`.
5.  **Deploy**: Pushes `public/` folder to `gh-pages` branch.
6.  **Post-Deploy**: Submits sitemap to IndexNow.

## ⚠️ Important Notes for Agents
- **Submodules**: Be aware that `content/` is a submodule. Changes there might need to be committed to the submodule repo first, or handled via the parent repo's update mechanism.
- **Theme Overrides**: To modify theme behavior, **do not edit `themes/` directly**. Copy the file to `layouts/` or `assets/` matching the theme's path to override it.
- **Config Split**: When adding configuration, choose the appropriate file in `config/_default/` rather than dumping everything in `config.toml`.
