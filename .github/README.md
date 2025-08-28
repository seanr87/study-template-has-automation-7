# Study Template GitHub Actions

This directory contains GitHub Actions workflows that are automatically included in all new study repositories created from this template.

## Factory Objective Update Workflow

The `factory-objective-update.yml` workflow automatically updates the Factory issue's Objective field when status-tracking issues in the study repository are closed.

### How it works:

1. **Triggers**: Runs when study status-tracking issues are closed
2. **Objective Logic**: When an issue is closed → sets Factory Objective to the next issue's objective (or "Complete" if final)
3. **Factory Detection**: Automatically finds the corresponding Factory issue by extracting the link from the study issue body
4. **Factory Integration**: Directly updates the Factory Portfolio project using GitHub GraphQL API
5. **Comments**: Adds informative comments to Factory issues documenting the milestone completion

### Required Secrets:

Each study repository needs the following secret configured:

- `FACTORY_ORG_TOKEN`: Personal Access Token with permissions to:
  - Read/write issues in the Factory repository
  - Read/write organization projects (Factory Portfolio)
  - Write comments on Factory issues

### Status Tracking Issues:

The workflow looks for issues with the `status-tracking` label and these titles:
- "1) Analysis Package Prototype"
- "2) Network Execution" 
- "3) Journal Submission"

### Factory Issue Detection:

The workflow finds the corresponding Factory issue by looking for a Factory issue link in the study issue body that matches:
```
https://github.com/{owner}/Factory/issues/{number}
```

This link is automatically added when status-tracking issues are created during study provisioning.

### Troubleshooting:

**Workflow not triggering:**
- Ensure the issue has the `status-tracking` label
- Verify the `FACTORY_ORG_TOKEN` secret is configured
- Check that the Factory issue link exists in the issue body

**Permission errors:**
- Ensure `FACTORY_ORG_TOKEN` has organization project read/write permissions
- Verify the token can access the Factory repository and organization projects

**Objective not updating:**
- Check that the Factory Portfolio project exists and has an 'Objective' field
- Verify the objective options match configuration: "Analysis Package Prototype", "Network Execution", "Journal Submission", "Complete"

## Maintenance:

- This workflow is template-based and will be copied to new study repositories
- Updates to this workflow should be made in the study-template repository
- Existing study repositories will need manual updates to get workflow changes