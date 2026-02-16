# Next Steps to Complete the Release

## ✅ What Has Been Done

The repository has been configured for automatic releases:

1. ✅ Release workflow exists at `.github/workflows/release.yml`
2. ✅ Version updated to `0.0.1-fork-4.4.0` in:
   - `manifest.json`
   - `package.json`
   - `versions.json`
3. ✅ Build tested locally - `main.js` generates successfully
4. ✅ Release tag `release-0.0.1-fork-4.4.0` created locally
5. ✅ Documentation created in `RELEASE-PROCESS.md`

## 🚀 Steps to Trigger the First Release

Since the tag cannot be pushed from this automated environment, you'll need to complete these steps manually:

### Option 1: Push the Existing Tag (Recommended)

The tag has already been created locally in your repository. To push it:

```bash
cd /path/to/your/local/obsidian-gemini
git fetch origin copilot/setup-github-workflow-for-release
git checkout copilot/setup-github-workflow-for-release
git pull origin copilot/setup-github-workflow-for-release
git push origin release-0.0.1-fork-4.4.0
```

### Option 2: Merge PR and Create Tag Fresh

If you prefer to merge the PR first, then create the tag:

1. **Merge this PR** into your main/master branch on GitHub

2. **Pull the changes locally**:

   ```bash
   git checkout master
   git pull origin master
   ```

3. **Verify the version** in `manifest.json` is `0.0.1-fork-4.4.0`

4. **Create and push the tag**:
   ```bash
   git tag -a release-0.0.1-fork-4.4.0 -m "Release 0.0.1-fork-4.4.0"
   git push origin release-0.0.1-fork-4.4.0
   ```

### What Happens Next

Once you push the tag, GitHub Actions will automatically:

1. ✅ Run the test suite
2. ✅ Build the plugin (`npm run build`)
3. ✅ Create a **draft** GitHub Release with:
   - Tag: `release-0.0.1-fork-4.4.0`
   - Files: `main.js`, `manifest.json`, `styles.css`

### Verify and Publish the Release

1. **Go to GitHub**: Navigate to `https://github.com/HipsterZipster/obsidian-gemini/actions`
   - You should see a workflow run for your tag
   - Wait for it to complete (typically 2-3 minutes)

2. **Check the Release**: Go to `https://github.com/HipsterZipster/obsidian-gemini/releases`
   - You should see a draft release

3. **Publish the Release**:
   - Click "Edit" on the draft release
   - Add release notes (describe what's in this fork version)
   - Click "Publish release"

## 🔧 One-Time Setup: Workflow Permissions

**IMPORTANT**: Ensure GitHub Actions has the necessary permissions:

1. Go to: `https://github.com/HipsterZipster/obsidian-gemini/settings/actions`
2. Scroll to "Workflow permissions"
3. Select **"Read and write permissions"**
4. Click **"Save"**

Without this, the workflow won't be able to create releases.

## 📦 Installing Your Fork

Once the release is published, anyone can install your fork:

1. Go to `https://github.com/HipsterZipster/obsidian-gemini/releases`
2. Download `main.js`, `manifest.json`, and `styles.css` from the latest release
3. Create folder: `[your-vault]/.obsidian/plugins/gemini-scribe/`
4. Place the 3 files directly in that folder
5. Restart Obsidian or reload plugins
6. Enable "Gemini Scribe" in Settings → Community plugins

## 🔄 Future Releases

For future releases, see the complete guide in `RELEASE-PROCESS.md`.

Quick version:

1. Update version numbers in `manifest.json`, `package.json`, and `versions.json`
2. Commit and push changes
3. Create and push a tag:
   ```bash
   git tag -a release-X.Y.Z -m "Release X.Y.Z"
   git push origin release-X.Y.Z
   ```
4. GitHub Actions will handle the rest!

## 📋 Verification Checklist

Before pushing the tag, verify:

- [ ] All changes are committed and pushed to GitHub
- [ ] Workflow permissions are set to "Read and write"
- [ ] The version in `manifest.json` matches your tag name
- [ ] You can build the plugin locally with `npm run build`
- [ ] You're ready to publish the draft release after it's created
