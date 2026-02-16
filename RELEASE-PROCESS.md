# Release Process for Forked Plugin

This document describes how to create releases for this forked version of the Obsidian Gemini plugin.

## Overview

This repository is configured with GitHub Actions to automatically create releases when you push a tag. The workflow:

1. Runs tests to ensure code quality
2. Builds the plugin (generates `main.js`)
3. Creates a GitHub Release with the required plugin files
4. Attaches `main.js`, `manifest.json`, and `styles.css` to the release

## Important Files for Obsidian Plugins

Obsidian plugins require these files to be in the plugin folder:

- **main.js** (required) - The compiled plugin code
- **manifest.json** (required) - Plugin metadata and version
- **styles.css** (optional) - Plugin styling
- **data.json** (optional) - Plugin data, created by Obsidian

These files must be placed directly in `.obsidian/plugins/gemini-scribe/` folder in your vault. Subdirectories will prevent Obsidian from recognizing the plugin.

## Creating a Release

### Prerequisites

1. Ensure you're on the main/master branch:

   ```bash
   git checkout master
   git pull origin master
   ```

2. Ensure all changes are committed and pushed

### Step 1: Update Version Numbers

Update the version in the following files to match your desired release version:

- `manifest.json` - Update the `version` field
- `package.json` - Update the `version` field
- `versions.json` - Add a new entry mapping your version to the minimum Obsidian version

Or use the npm version command (recommended):

```bash
npm version patch  # For bug fixes (0.0.1 -> 0.0.2)
npm version minor  # For new features (0.0.1 -> 0.1.0)
npm version major  # For breaking changes (0.0.1 -> 1.0.0)
```

Note: The npm version command automatically updates `package.json`, `manifest.json`, and `versions.json`, creates a commit, and creates a tag.

### Step 2: Test the Build Locally

Before creating a release, test that the plugin builds successfully:

```bash
npm install
npm run build
```

This should generate a `main.js` file in the root directory.

### Step 3: Create and Push the Tag

The tag name should match the version in `manifest.json`. For fork releases, you can use a format like `release-X.Y.Z-fork-A.B.C`:

```bash
git tag -a release-0.0.1-fork-4.4.0 -m "Release 0.0.1-fork-4.4.0"
git push origin release-0.0.1-fork-4.4.0
```

The `-a` flag creates an annotated tag, and `-m` specifies the tag message.

### Step 4: Monitor the GitHub Actions Workflow

1. Go to your repository on GitHub
2. Click the "Actions" tab
3. You should see a workflow run for your tag
4. Wait for the workflow to complete (typically 2-3 minutes)

### Step 5: Publish the Release

Once the workflow completes:

1. Go to your repository's main page on GitHub
2. Click "Releases" in the right sidebar
3. You should see a draft release with your tag name
4. Click "Edit" (pencil icon) on the draft release
5. Add release notes describing the changes
6. Click "Publish release"

## Installing the Plugin from a Release

To install this forked plugin in Obsidian:

1. Go to the "Releases" page of this repository
2. Download the following files from the latest release:
   - `main.js`
   - `manifest.json`
   - `styles.css`
3. Create a new folder in your vault: `.obsidian/plugins/gemini-scribe/`
4. Place the downloaded files directly in this folder
5. Restart Obsidian or reload the plugin
6. Enable the plugin in Settings → Community plugins

## Automation Details

The release process is automated via `.github/workflows/release.yml`:

- **Trigger**: Pushes to any tag (`*`)
- **Test Job**: Runs `npm test` to ensure code quality
- **Build Job**:
  - Installs dependencies
  - Builds the plugin with `npm run build`
  - Creates a draft GitHub release
  - Uploads `main.js`, `manifest.json`, and `styles.css`

## Notes for Fork Maintainers

- This fork is **NOT** published to the official Obsidian Community Plugins directory
- Users must manually install from GitHub releases
- Consider using a version scheme that differentiates from the upstream plugin (e.g., `0.0.x-fork-y.y.y`)
- The `main.js` file is excluded from git (see `.gitignore`) and is built during the release process
- Always test locally before creating a release tag

## Troubleshooting

### Workflow fails with "permission denied"

Ensure GitHub Actions has write permissions:

1. Go to repository Settings
2. Click "Actions" → "General" in the left sidebar
3. Scroll to "Workflow permissions"
4. Select "Read and write permissions"
5. Click "Save"

### Build fails with missing dependencies

Run `npm install` to ensure all dependencies are present before building.

### Tag already exists

If you need to recreate a tag:

```bash
git tag -d release-0.0.1-fork-4.4.0          # Delete locally
git push origin :refs/tags/release-0.0.1-fork-4.4.0  # Delete remotely
# Then create the tag again
```
