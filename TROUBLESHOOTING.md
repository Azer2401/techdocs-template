# Troubleshooting GitHub Form Errors in Backstage Templates

## Common Issues and Solutions

### 1. GitHub Authentication Issues

**Problem**: Template fails to authenticate with GitHub
**Solution**: 
- Ensure GitHub integration is properly configured in your Backstage config
- Check that the GitHub token has the necessary permissions (repo, user:email)
- Verify the token is not expired

### 2. Template URL Issues

**Problem**: Template cannot fetch skeleton files
**Solution**:
- Use absolute URLs instead of relative paths
- Ensure the repository is publicly accessible or the token has access
- Check that the skeleton directory exists in the specified path

### 3. Catalog Registration Issues

**Problem**: Failed to register component in catalog
**Solution**:
- Verify catalog configuration in Backstage
- Check that the catalog-info.yaml file is properly formatted
- Ensure the catalog has write permissions

### 4. Repository Access Issues

**Problem**: Cannot access target repository
**Solution**:
- Ensure the GitHub token has access to the target repository
- Check repository permissions and visibility settings
- Verify the repository URL format is correct

## Configuration Checklist

### Backstage Config Requirements

1. **GitHub Integration**:
```yaml
integrations:
  github:
  - host: github.com
    token: ${GITHUB_TOKEN}
```

2. **Catalog Configuration**:
```yaml
catalog:
  import:
    entityFilename: catalog-info.yaml
    pullRequestBranchName: backstage-integration
  locations:
  - target: https://github.com/Azer2401/techdocs-template/blob/main/location.yaml
    type: url
```

3. **Scaffolder Configuration**:
```yaml
scaffolder:
  github:
    token: ${GITHUB_TOKEN}
    visibility: public
```

### Environment Variables

Ensure these environment variables are set:
- `GITHUB_TOKEN`: GitHub personal access token with repo permissions
- `POSTGRES_HOST`, `POSTGRES_PASSWORD`, `POSTGRES_PORT`, `POSTGRES_USER`: Database connection
- `CLIENT_ID`, `CLIENT_SECRET`: Keycloak authentication

## Debugging Steps

1. **Check Backstage Logs**:
```bash
kubectl logs -f deployment/backstage-app
```

2. **Verify Template Registration**:
- Check if templates appear in the Backstage UI
- Verify template YAML syntax is correct

3. **Test GitHub Integration**:
- Try creating a simple test repository
- Check if GitHub API calls are successful

4. **Validate Catalog Info**:
- Ensure catalog-info.yaml files are properly formatted
- Check that all required fields are present

## Template Fixes Applied

### 1. Removed User Credentials Request
- Removed `requestUserCredentials` from RepoUrlPicker
- This was causing authentication issues in the GitHub form

### 2. Updated Template URLs
- Changed from relative paths (`./skeleton`) to absolute URLs
- Now using: `https://github.com/Azer2401/techdocs-template/software-techdocs/skeleton`

### 3. Fixed Catalog Info Files
- Added proper Kubernetes annotations handling
- Ensured consistent formatting across templates

## Testing the Fix

1. **Deploy Updated Templates**:
```bash
# Apply the updated templates
kubectl apply -f software-techdocs/template.yaml
kubectl apply -f hardware-techdocs/template.yaml
```

2. **Test Template Creation**:
- Go to Backstage UI
- Navigate to Create > Software Documentation Template
- Fill out the form and test the creation process

3. **Monitor Logs**:
```bash
kubectl logs -f deployment/backstage-app -c backstage
```

## Common Error Messages

### "Failed to fetch template"
- Check template URL accessibility
- Verify GitHub token permissions

### "Repository not found"
- Ensure repository exists and is accessible
- Check GitHub token has repo access

### "Catalog registration failed"
- Verify catalog configuration
- Check catalog-info.yaml format

### "Authentication failed"
- Verify GitHub token is valid
- Check token permissions and expiration

## Additional Configuration

If you're still experiencing issues, consider these additional configurations:

### 1. Enhanced GitHub Integration
```yaml
integrations:
  github:
  - host: github.com
    token: ${GITHUB_TOKEN}
    apiBaseUrl: https://api.github.com
```

### 2. Scaffolder Permissions
```yaml
permission:
  enabled: true
  policies:
    - policy: 'allow-all'
      resource: 'template'
```

### 3. Catalog Rules
```yaml
catalog:
  rules:
  - allow:
    - Component
    - System
    - API
    - Resource
    - Location
    - Template
```

## Support

If you continue to experience issues:

1. Check the Backstage community forums
2. Review the official Backstage documentation
3. Create an issue in the Backstage GitHub repository
4. Contact your Backstage administrator

---

**Note**: This troubleshooting guide is specific to the techdocs-template configuration. Adjust paths and URLs according to your specific setup.
