# Working with Azure DevOps

This guide explains how to fork, import, and work with the API Management Developer Portal repository using Azure DevOps.

## Importing from GitHub to Azure DevOps

Azure DevOps doesn't have a traditional "fork" feature like GitHub, but you can import the repository into Azure DevOps Repos:

### Method 1: Import Repository (Recommended)

1. **Navigate to Azure DevOps Repos**
   - Go to your Azure DevOps organization (https://dev.azure.com/yourorganization)
   - Select your project or create a new one

2. **Import the Repository**
   - In your project, go to **Repos** > **Files**
   - Click the repository dropdown at the top
   - Select **Import repository**
   - Enter the clone URL: `https://github.com/Azure/api-management-developer-portal.git` (or this fork's URL)
   - Name your repository
   - Click **Import**

3. **Set Up Authentication (if repository is private)**
   - Select **Requires authorization**
   - Provide your GitHub username and a Personal Access Token (PAT)
   - To create a GitHub PAT:
     - Go to GitHub Settings > Developer settings > Personal access tokens
     - Generate a token with `repo` scope
     - Use this token for authentication

### Method 2: Git Clone and Push

If you prefer working with Git commands:

```bash
# Clone the repository locally
git clone https://github.com/Azure/api-management-developer-portal.git
cd api-management-developer-portal

# Add Azure DevOps remote
git remote add azure https://dev.azure.com/yourorganization/yourproject/_git/api-management-developer-portal

# Push to Azure DevOps
git push azure --all
git push azure --tags
```

## Setting Up Azure Pipelines

The repository includes an Azure DevOps pipeline configuration for migration tasks:

1. **Create a Pipeline**
   - Go to **Pipelines** > **Pipelines**
   - Click **New pipeline**
   - Select **Azure Repos Git** (if imported) or **GitHub** (if using GitHub as source)
   - Select your repository
   - Choose **Existing Azure Pipelines YAML file**
   - Select the `.pipeline/migrate.yml` file
   - Click **Continue**

2. **Configure Pipeline Variables**
   
   Set up the following variables (mark sensitive ones as secret):
   
   | Variable Name | Description | Secret |
   |---------------|-------------|--------|
   | sourceSubscriptionId | Source APIM service subscription ID | No |
   | sourceResourceGroupName | Source APIM service resource group name | No |
   | sourceServiceName | Source APIM service name | No |
   | destServiceName | Destination APIM service name | No |
   | destSubscriptionId | Destination APIM service subscription ID | No |
   | destResourceGroupName | Destination APIM service resource group name | No |
   | sourceAzureTenantId | Source Azure tenant ID | No |
   | sourceServicePrincipal | Source Azure service principal | Yes |
   | sourceServicePrincipalSecret | Source Azure service principal secret | Yes |
   | destAzureTenantId | Destination Azure tenant ID | No |
   | destServicePrincipal | Destination Azure service principal | Yes |
   | destServicePrincipalSecret | Destination Azure service principal secret | Yes |
   | existingEnvUrls | Existing environment URLs (comma-separated) | No |
   | destEnvUrls | Destination environment URLs (comma-separated) | No |

   See [.pipeline/readme.md](.pipeline/readme.md) for more details.

3. **Run the Pipeline**
   - Click **Run** to save and execute your pipeline

## Development in Azure DevOps

### Setting Up Development Environment

1. **Clone Your Azure DevOps Repository**
   ```bash
   git clone https://dev.azure.com/yourorganization/yourproject/_git/api-management-developer-portal
   cd api-management-developer-portal
   ```

2. **Install Dependencies**
   ```bash
   npm install
   ```

3. **Start Development Server**
   ```bash
   npm start
   ```

### Creating Pull Requests

In Azure DevOps, Pull Requests work similarly to GitHub:

1. Create a new branch for your changes
2. Make your changes and commit them
3. Push the branch to Azure DevOps
4. Go to **Repos** > **Pull requests**
5. Click **New pull request**
6. Select your source and target branches
7. Add reviewers and complete the PR details

## Keeping Your Fork Updated

To sync your Azure DevOps repository with the original GitHub repository:

### Using Git Commands

```bash
# Add the original repository as upstream (if not already added)
git remote add upstream https://github.com/Azure/api-management-developer-portal.git

# Fetch upstream changes
git fetch upstream

# Merge upstream changes into your main branch
git checkout master
git merge upstream/master

# Push to Azure DevOps
git push origin master
```

### Using Azure DevOps UI

Unfortunately, Azure DevOps doesn't have a built-in sync feature like GitHub. You'll need to use Git commands or set up an automated pipeline to sync changes.

## Working with Both GitHub and Azure DevOps

If you want to maintain both GitHub and Azure DevOps remotes:

```bash
# Add both remotes
git remote add github https://github.com/yourusername/api-management-developer-portal.git
git remote add azure https://dev.azure.com/yourorganization/yourproject/_git/api-management-developer-portal

# Push to both
git push github master
git push azure master

# Or push to all remotes at once
git remote set-url --add --push origin https://github.com/yourusername/api-management-developer-portal.git
git remote set-url --add --push origin https://dev.azure.com/yourorganization/yourproject/_git/api-management-developer-portal
git push origin master  # This will push to both
```

## Additional Resources

- [Azure DevOps Repos Documentation](https://docs.microsoft.com/en-us/azure/devops/repos/)
- [Azure Pipelines Documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/)
- [API Management Developer Portal Documentation](https://aka.ms/apimdocs/portal)
- [Pipeline Configuration](.pipeline/readme.md)

## Support

For Azure DevOps specific issues:
- [Azure DevOps Community](https://developercommunity.visualstudio.com/spaces/21/index.html)
- [Stack Overflow - azure-devops tag](https://stackoverflow.com/questions/tagged/azure-devops)

For portal-related issues, refer to the [README.md](README.md) for support channels.
