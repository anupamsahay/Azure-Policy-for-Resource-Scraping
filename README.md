I've created a comprehensive Azure IAM policy specifically tailored for all the resource types you requested. This policy provides complete read-only access for cost optimization analysis and resource consistency checking across your entire Azure infrastructure.
Key Features:
Coverage Includes:

Compute & Infrastructure: Virtual Machines, VM Licenses, Container Apps, AKS, Functions, Container Instances
Storage & Data: Storage Accounts, NetApp Files, Cosmos DB, Databricks
Databases: SQL Database, SQL Managed Instance, PostgreSQL, Arc-enabled databases
Networking: Virtual Networks, ExpressRoute, Load Balancer, NAT Gateway, Firewall, VPN Gateway, Application Gateway, Route Server, DNS
Analytics & AI: Synapse Analytics, Analysis Services, Log Analytics, Cognitive Services, Cognitive Search, Grafana
Integration & Workflow: Data Factory v2, Logic Apps, Event Grid, Event Hubs, Service Bus, API Management
Developer Tools: DevOps, GitHub integration, Container Registry
Security & Compliance: Defender for Cloud, Security Copilot, Key Vault, Network Watcher
Management: Backup, Monitor, Automation, Advisor, Intelligent Recommendations
Specialized Services: Microsoft Fabric, Communication Services (Phone/Voice), Entra Domain Services, SaaS
Hybrid & Edge: Azure Arc, Arc-enabled databases, Azure Stack HCI (Azure Local)

Special Permissions:

Storage Account Keys: Can list keys for detailed cost analysis
Metrics & Diagnostics: Full read access to all monitoring data
Tags & Resource Groups: Complete visibility for organizational analysis
Cost Management: Direct access to billing, consumption, and cost data

Security Features:

Strictly read-only with explicit blocks on write/delete operations
No privilege escalation possible
DataActions included for reading blob/file contents when needed for analysis
Protected against authorization changes

Implementation Notes:

Replace {guid} with a unique GUID (use New-Guid in PowerShell)
Replace {subscription-id}, {resource-group-name}, and {management-group-id} with your actual values
Comments are included (lines starting with //) for clarity but should be removed before deployment as JSON doesn't support comments
The wildcard */read at the end ensures future service coverage

This policy enables comprehensive resource inspection capabilities needed for cost optimization tools, compliance checking, and resource consistency analysis while maintaining strict security boundaries.RetryClaude can make mistakes. Please double-check responses.



========


# Azure IAM Policy Execution Guide

## Quick Execution Steps

### Step 1: Prepare the JSON File

#### 1.1 Save the JSON
Create a new file named `resource-inspection-role.json` and copy the policy JSON into it.

#### 1.2 Clean the JSON
Remove ALL comment lines (lines containing `//`) as JSON doesn't support comments.

**Original (with comments):**
```json
"Actions": [
    "// Core Management and Cost Analysis",
    "Microsoft.Advisor/*/read",
```

**Cleaned (ready for deployment):**
```json
"Actions": [
    "Microsoft.Advisor/*/read",
```

#### 1.3 Generate Required IDs
Open PowerShell or Terminal and generate a GUID:

**Windows PowerShell:**
```powershell
[guid]::NewGuid()
```

**Mac/Linux:**
```bash
uuidgen
```

Example output: `a1b2c3d4-e5f6-7890-abcd-ef1234567890`

#### 1.4 Replace Placeholders
Find and replace these values in your JSON:

| Placeholder | Replace With | Example |
|------------|--------------|---------|
| `{guid}` | Your generated GUID | `a1b2c3d4-e5f6-7890-abcd-ef1234567890` |
| `{subscription-id}` | Your Azure Subscription ID | `12345678-1234-1234-1234-123456789012` |
| `{resource-group-name}` | Your Resource Group name | `rg-production` |
| `{management-group-id}` | Your Management Group ID | `mg-enterprise` |

### Step 2: Get Your Azure Subscription ID

**Option A: Azure Portal**
1. Login to [Azure Portal](https://portal.azure.com)
2. Search "Subscriptions" in the top search bar
3. Copy your Subscription ID

**Option B: Command Line**
```bash
# Azure CLI
az account show --query id -o tsv

# PowerShell
Get-AzSubscription | Select-Object Name, Id
```

### Step 3: Execute the Script

## Method 1: Azure Portal (Easiest - No Installation Required)

1. **Login to Azure Portal**
   - Go to https://portal.azure.com
   - Sign in with your Azure account

2. **Open Cloud Shell**
   - Click the shell icon `>_` in the top menu bar
   - Choose **Bash** when prompted
   - Wait for the shell to initialize

3. **Create the JSON file**
   ```bash
   # Create and open the file in the editor
   code resource-inspection-role.json
   ```
   
4. **Paste your cleaned JSON**
   - Paste the entire JSON (without comments)
   - Press `Ctrl+S` (or `Cmd+S` on Mac) to save
   - Press `Ctrl+Q` (or `Cmd+Q` on Mac) to close editor

5. **Execute the deployment command**
   ```bash
   # Deploy the custom role
   az role definition create --role-definition resource-inspection-role.json
   ```

6. **Verify deployment**
   ```bash
   # Check if role was created successfully
   az role definition list --name "Resource-Inspection-CostOptimization-Comprehensive-Role" --output table
   ```

## Method 2: Local Azure CLI

### Prerequisites Installation

**Windows:**
```powershell
# Install Azure CLI using MSI installer
# Download from: https://aka.ms/installazurecliwindows
# Or use PowerShell:
Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi
Start-Process msiexec.exe -ArgumentList '/I AzureCLI.msi /quiet' -Wait
```

**Mac:**
```bash
# Install using Homebrew
brew update && brew install azure-cli
```

**Linux (Ubuntu/Debian):**
```bash
# Install using apt
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

### Execute the Script

1. **Login to Azure**
   ```bash
   # Login (will open browser for authentication)
   az login
   
   # If you have multiple subscriptions, set the correct one
   az account set --subscription "YOUR-SUBSCRIPTION-NAME-OR-ID"
   ```

2. **Navigate to your JSON file location**
   ```bash
   cd /path/to/your/json/file
   ```

3. **Deploy the role**
   ```bash
   az role definition create --role-definition resource-inspection-role.json
   ```

## Method 3: PowerShell

1. **Install Azure PowerShell module**
   ```powershell
   # Install the module
   Install-Module -Name Az -Repository PSGallery -Force
   
   # Import the module
   Import-Module Az
   ```

2. **Login and deploy**
   ```powershell
   # Login to Azure
   Connect-AzAccount
   
   # Set subscription if needed
   Set-AzContext -SubscriptionId "YOUR-SUBSCRIPTION-ID"
   
   # Deploy the custom role
   New-AzRoleDefinition -InputFile "resource-inspection-role.json"
   ```

## Method 4: Direct REST API Call

For automation or CI/CD pipelines:

```bash
# Get access token
ACCESS_TOKEN=$(az account get-access-token --query accessToken -o tsv)

# Your subscription ID
SUBSCRIPTION_ID="YOUR-SUBSCRIPTION-ID"
ROLE_ID="YOUR-GENERATED-GUID"

# Make the REST API call
curl -X PUT \
  "https://management.azure.com/subscriptions/${SUBSCRIPTION_ID}/providers/Microsoft.Authorization/roleDefinitions/${ROLE_ID}?api-version=2022-04-01" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -H "Content-Type: application/json" \
  -d @resource-inspection-role.json
```

## Step 4: Assign the Role to Users

### Quick Assignment Commands

**Assign to a user:**
```bash
az role assignment create \
    --role "Resource-Inspection-CostOptimization-Comprehensive-Role" \
    --assignee "user@yourdomain.com" \
    --scope "/subscriptions/YOUR-SUBSCRIPTION-ID"
```

**Assign to a service principal (for automation):**
```bash
# First create a service principal
az ad sp create-for-rbac --name "cost-optimization-sp" --skip-assignment

# Then assign the role
az role assignment create \
    --role "Resource-Inspection-CostOptimization-Comprehensive-Role" \
    --assignee "SERVICE-PRINCIPAL-APP-ID" \
    --scope "/subscriptions/YOUR-SUBSCRIPTION-ID"
```

**Assign to an Azure AD group:**
```bash
# Get the group's object ID
GROUP_ID=$(az ad group show --group "YourGroupName" --query objectId -o tsv)

# Assign the role
az role assignment create \
    --role "Resource-Inspection-CostOptimization-Comprehensive-Role" \
    --assignee-object-id "$GROUP_ID" \
    --assignee-principal-type Group \
    --scope "/subscriptions/YOUR-SUBSCRIPTION-ID"
```

## Step 5: Verify Everything is Working

### Test Commands
Run these commands to verify the role is working:

```bash
# 1. Check role exists
az role definition list --name "Resource-Inspection-CostOptimization-Comprehensive-Role"

# 2. Check who has the role assigned
az role assignment list --role "Resource-Inspection-CostOptimization-Comprehensive-Role" --output table

# 3. Test permissions (as the assigned user)
az vm list --output table
az storage account list --output table
az sql server list --output table

# 4. Verify read-only (this should fail)
az vm stop --name test-vm --resource-group test-rg
# Expected: "AuthorizationFailed" error
```

## Common Errors and Quick Fixes

### Error: "Unexpected token '//' in JSON"
**Fix:** Remove all comment lines containing `//`

### Error: "Invalid JSON format"
**Fix:** Validate your JSON at https://jsonlint.com/

### Error: "The subscription 'xxx' cannot be found"
**Fix:** 
```bash
# List available subscriptions
az account list --output table
# Set the correct one
az account set --subscription "CORRECT-SUBSCRIPTION-ID"
```

### Error: "AuthorizationFailed"
**Fix:** Ensure you have Owner or User Access Administrator role:
```bash
# Check your permissions
az role assignment list --assignee $(az account show --query user.name -o tsv) --output table
```

### Error: "RoleDefinitionLimitExceeded"
**Fix:** Delete unused custom roles:
```bash
# List all custom roles
az role definition list --custom-role-only true --output table
# Delete an unused role
az role definition delete --name "OLD-ROLE-NAME"
```

## One-Line Deployment (For Experienced Users)

If you have the JSON file ready and cleaned:

**Bash/Cloud Shell:**
```bash
az login && az role definition create --role-definition resource-inspection-role.json && echo "Role deployed successfully!"
```

**PowerShell:**
```powershell
Connect-AzAccount; New-AzRoleDefinition -InputFile "resource-inspection-role.json"; Write-Host "Role deployed successfully!" -ForegroundColor Green
```

## Validation Checklist

- [ ] JSON file has no comments (`//` lines removed)
- [ ] GUID generated and replaced in JSON
- [ ] Subscription ID added to AssignableScopes
- [ ] Logged into correct Azure subscription
- [ ] Role created successfully (verify with list command)
- [ ] Role assigned to at least one user/group/service principal
- [ ] Test user can read resources
- [ ] Test user cannot modify resources (read-only verification)

## Next Steps

1. **Document the role assignment** for your team
2. **Set up monitoring** for role usage:
   ```bash
   az monitor activity-log list --resource-type "Microsoft.Authorization/roleAssignments" --output table
   ```
3. **Schedule regular audits** of who has this role:
   ```bash
   az role assignment list --role "Resource-Inspection-CostOptimization-Comprehensive-Role" > role-audit-$(date +%Y%m%d).txt
   ```

## Support Commands

**Get help:**
```bash
az role definition create --help
az role assignment create --help
```

**Enable debug logging:**
```bash
az role definition create --role-definition resource-inspection-role.json --debug
```

**Check Azure service health:**
```bash
az rest --method get --url "https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ResourceHealth/availabilityStatuses?api-version=2022-10-01"
```


========
