# Configure Azure resources with tools

## Azure Portal

The Azure portal lets you build, manage, and monitor everything from simple web apps to complex cloud applications in a single, unified console.

**Features**
- Search resources, services, and docs.
- Manage resources.
- Create customized dashboards and favorites.
- Access the Cloud Shell.
- Receive notifications.
- Links to the Azure documentation.

Link: [https://portal.azure.com](https://portal.azure.com).

## Azure Cloud Shell

Azure Cloud Shell is an interactive, browser-accessible shell for managing Azure resources.

Supports Bash and PowerShell experience.

**Features**

- Is temporary and requires a new or existing Azure Files share to be mounted.
- Offers an integrated graphical text editor based on the open-source Monaco Editor.
- Authenticates automatically for instance access to your resources.
- Runs on a temporary host provided on a per-session, per-user basis.
- Times out after 20 minutes without interactive activity.
- Requires a resources group, storage account, and Azure File share.
- Uses the same Azure file share for both Bash and PowerShell.
- Is assigned to one machine per user account.
- Persists $HOME using a 5GB image held in your file share.
- Permissions are set as a regular Linux user in Bash.

## Azure PowerShell

Azure PowerShell is a module that you add to Windows PowerShell or PowerShell Core to enable you to connect to your Azure subscription and manage resources.

Azure PowerShell is available inside a browser via the Azure Cloud Shell, or with a local installation on Linux, macOS, or the Windows operating system. You can use it in interactive or scripting mode.

Az is the formal name for the Azure PowerShell open-source module containing cmdlets to work with Azure features.

## Azure CLI

Azure CLI is a command-line program to connect to Azure and execute administrative commands on Azure resources. It runs on Linux, macOS, and Windows, and allows administrators and developers to execute their commands through a terminal, command-line prompt, or script instead of a web browser.

Azure CLI can be used both interactively or through scripts (using the syntax of your chosen shell).

Commands in the CLI are structured in groups or subgroups. Each group represents a service provided by Azure, and the subgroups divide commands for these services into logical groupings.

You can use `az find` to find particular commands you need.

```bash
az find blob
```

If you already know the name of the command but need detailed information, use `--help`:

```bash
az storage blob --help
```

# Use Azure Resource Manager

