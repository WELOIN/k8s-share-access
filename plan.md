# k8s-share-access Improvement Plan

## Token Expiration & Rotation
- [x] Add option to set custom token expiration time
- [ ] Add "refresh token" command to regenerate tokens for existing users

## User Management Enhancements
- [x] Edit user - Modify existing user's namespace access without recreating
- [ ] Show user details - View detailed permissions for a specific user
- [ ] Export user list - Export all users to CSV/JSON

## Namespace Groups / Templates
- [ ] Pre-defined namespace groups (e.g., "dev-team" = dev, staging, qa)
- [ ] Save and reuse permission templates

## Kubeconfig Management
- [ ] Merge kubeconfig - Option to merge into existing kubeconfig instead of separate file
- [ ] Set expiration reminder - Add comment with token expiration date in generated file
- [ ] Base64 encode - Option to output base64-encoded kubeconfig for easy sharing

## Security Improvements
- [ ] Audit log - Log all user creation/deletion actions
- [ ] Token rotation policy - Warn about old tokens
- [ ] Restrict secret access - Option to exclude secrets from read permissions

## Bulk Operations
- [ ] Create multiple users from a YAML/JSON file
- [ ] Delete all users matching a pattern

## Better Feedback
- [ ] Dry-run mode - Show what would be created without applying
- [ ] Verbose mode - Show exact kubectl commands being run
- [ ] Test connection - Verify generated kubeconfig works before saving

## Additional Access Patterns
- [x] Port forwarding - Optional port-forward permission for create and edit flows
- [ ] Time-limited access - Auto-cleanup after X days
- [ ] Custom role support - Use existing roles instead of creating new ones
- [ ] Resource quotas - Optionally set resource quotas per user

## Code Improvements
- [ ] Add `--non-interactive` mode for CI/CD pipelines
- [ ] Add `--config` flag to read options from file
- [ ] Add version flag (`--version`)
