# GitHub Auto-Sync Workflow

This repository includes an automated GitHub Actions workflow that keeps your fork synchronized with the upstream repository while preserving your custom files.

## What does it do?

🔄 **Automatic Updates**: Runs daily at midnight UTC to check for new code from the original repository  
📦 **Release Sync**: Automatically copies new releases from upstream to your repository  
🛡️ **File Protection**: Preserves your custom `README.md` while updating all other code  
📝 **Documentation**: Creates `INTRO.md` with the original repository's readme for reference  

## Benefits

- ✅ Always have the latest features and bug fixes
- ✅ Your users get the same release versions as the original
- ✅ Keep your custom documentation and setup instructions
- ✅ No manual work required - fully automated
- ✅ Can be triggered manually when needed

## How to set it up

### 1. Create the workflow file
Create this file structure in your repository:
```
.github/
└── workflows/
    └── sync.yml
```

### 2. Add the workflow content
Copy the auto-sync workflow code into `.github/workflows/sync.yml`

### 3. Commit and push
```bash
git add .github/workflows/sync.yml
git commit -m "Add auto-sync workflow"
git push
```

### 4. Verify it's working
- Go to your repository on GitHub
- Click the **Actions** tab
- You should see "Auto Sync with Upstream" workflow
- Click "Run workflow" to test it manually

## File locations

- **Workflow file**: `.github/workflows/sync.yml`
- **Your custom README**: `README.md` (protected, won't be overwritten)
- **Original README**: `INTRO.md` (auto-updated from upstream)

## Schedule

The workflow runs automatically:
- **Daily** at midnight UTC
- **Manually** via GitHub Actions tab (click "Run workflow")

## Troubleshooting

If the workflow fails:
1. Check the Actions tab for error messages
2. Ensure the upstream repository URL is correct
3. Verify your repository has proper permissions

---

*This automation ensures your fork stays current while maintaining your customizations.*