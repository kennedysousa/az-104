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

The infrastructure for your solutions is typically made up of many components. There components are not separate entities, instead they are related and interdependent parts of a single entity. You want to deploy, manage, and monitor them as a group.

Azure REsource Manager enables you to work with the resources in your solution as a group. You can deploy, update, or delete all the resources for your solution in a single, coordinated operation. You use a template for deployment and that template can work for different environments such as testing, staging, and production.

Azure Resource Manager provides security, auditing, and tagging features to help you manage your resources after deployment.

## Benefits

Azure Resource Manager provides several benefits:

- You can deploy, manage, and monitor all the resources for your **solution as a group**.
- You can repeatedly deploy your solution throughout the development lifecycle and have confidence that your resources are **deployed consistently**.
- You can manage your infrastructure through **declarative templates** rather than scripts.
- You can define the dependencies between resources so they are deployed in the correct order.
- RBAC is natively integrated into the management platform.
- You can apply tags to resources to logically organize all the resources in your subscription.
- You can clarify your organization's billing by viewing costs for a group of resources sharing the same tag.

## Guidance

- Define and deploy your infrastructure through the declarative syntax in Azure Resource Manager templates, rather than through imperative commands.
- Define all deployment and configuration steps in the template. You should have no manual steps for setting up your solution.
- Run imperative commands to manage your resources, such as to start or stop an app or machine.
- Arrange resources with the same lifecycle in a resource group.
- Use tags for organizing the resources.

## Review Azure resource terminology

- **resource:** a manageable item that is available through Azure, such as VM, storage account, web app, database, virtual network, and many others.
- **resource group:** a container that holds related resources for an Azure solution. It can include all the resources for a solution, or only those resources that you want to manage as a group.
- **resource provider:** a service that supplies the resources you can deploy and manage through Resource Manager, such as Microsoft.Compute, Microsoft.Storage, and Microsoft.Web.
- **template:** A JSON file that defines one or more resources to deploy to a resource group. It also defines the dependencies between the deployed resources.

### Resource providers

Each resource provider offers a set of resources and operations for working with an Azure service.

The name of a resource type is in the format {resource-provider}/{resource-type}, e.g. `Microsoft.KeyVault/vaults`.

## Create resource groups

Resources can be deployed to any new or existing resource groups. Deployments are increamental - if a resource group contains two web apps and you decide to deploy a third, the existing web apps will not be removed.

### Considerations

Resource Groups are at their simplest a logical collection of resources. There are a few rules for resource groups.

- Resources can only exist in one resource group.
- Resource Groups cannot be renamed.
- Resource Groups can have resources of many different types (services).
- Resource Groups can have resources from many different regions.

### Creating resource groups

There are some important factors to consider when defining your resource group:

- All the resources in your group should share the same lifecycle. You deploy, update, and delete them together. If one resource, such as a database server, needs to exist on a different deployment cycle it should be in another resource group.
- Each resource can only exist in one resource group.
- You can add or remove a resource to a resource group at any time.
- You can move a resource from one resource group to another group. Limitations do apply to moving resources.
- A resource group can contain resources that reside in different regions.
- A resource group can be used to scope access control for administrative actions.
- A resource can interact with resources in other resource groups. This interaction is common when the two resources are related but don't share the same lifecycle (for example, web apps connecting to a database).

When creating a resource group you must provide a location for that resource group (i.e. where the metadata about that resource is stored). The resources can have different locations that the resource group.

> **Note:** by scoping permissions to a resource group, you can add/remove and modify resources easily without having to recreate assignments and scopes.

## Create Azure Resource Manager locks

A common concern with resources provisioned in Azure is the ease with which they can be deleted. Resource Manager locks allow organizations to put a structure in place that prevents the accidental deletion of resources in Azure.

- You can associate the lock with a subscription, resource group, or resource.
- Locks are inherited by child resources.

There are two types of resource locks:

- **Read-Only locks**, which prevent any changes to the resource.
- **Delete locks**, which prevent deletion.

> **Note:** only the *Owner* and *User Access Administrator* roles can create or delete management locks.

## Reorganize Azure resources

Sometimes you may need to move resources to either a new subscription or a new resource group in the same subscription.

When moving resources, both the source group and the target group are locked during the operation. Write and delete operations are blocked on the resource groups until the move completes. This lock means you can't add, update, or delete resources in the resource groups. Locks don't mean the resources aren't available. For example, if you move a virtual machine to a new resource group, an application can still access the virtual machine.

### Implementation

To move resources, select the resource group containing the resources, and then select the **Move** button. Select the resources to move and the destination resource group. Acknowledge that you need to update scripts.

> **Note:** just because a service can be moved doesn't mean there aren't restrictions. For example, you can move a virtual network, but you must also move its dependent resources, like gateways.

### Remove resources and resource groups

Use caution when deleting a resource group. Deleting a resource group deletes all the resources contained within it. That resource group might contain resources that resources in other resource groups depend on.

To remove a resource group using PowerShell, use the cmdlet `Remove-AzResourceGroup -Name <name>`.

You can also delete individual resources within a resource group or move them to another resource group.

## Determine resource limits

Azure lets you view resource usage agains limits. This is helpful to track current usave, and plan for future use.

- The limits shown are the limits for your subscription.
- When you need to increase a default limit, there is a Request Increase link.
- All resources have a maximum limit listed in Azure limits.
- If you are at the maximum limit, the limit can't be increased.

# Configure resources with Azure Resource Manager templates

## Review Azure Resource Manager template advantages

An Azure Resource Manager template precisely defines all the Resource Manager resources in a deployment. You can deploy a Resource Manager template into a resource group as a single operation.

Using Resource Manager templates will make your deployments faster and more repeatable.

### Templates benefits

- Improve consistency
- Help express complex deployments
- Reduce manual, error-prone tasks
- Can be shared, tested, and versioned as code
- Promote reuse
- Are linkable
- Simplify orchestration

## Explore the Azure Resource Manager template schema

Azure Resource Manager templates are written in JSON, which allows you to express data stored as an object in text.

A JSON document is essentially a collection of key-value pairs. Each key is a string, whose value can be a string, a number, a Boolean expression, a list of values, or an object.

A Resource Manager template can contain sections that are expressed using JSON notation, but aren't related to the JSON language itself:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {},
    "variables": {},
    "functions": [],
    "resources": [],
    "outputs": {}
}
```

| Element name | Required | Description |
| ------------ | -------- | ----------- |
| $schema | Yes | Location of the JSON schema file that describes the version of the template language. |
| contentVersion | Yes | Version of the template (such as 1.0.0.0). You can provide any value for this element. Use this value to document significant changes in your template. This value can be used to make sure that the right template is being used.|
| parameters | No | Values that are provided when deployment is executed to customize resource deployment. |
| variables | No | Values that are used as JSON fragments in the template to simplify template language expressions. |
| functions | No | User-defined functions that are available within the template. |
| resources | Yes | Resource types that are deployed or updated in a resource group. |
| outputs | No | Values that are returned after deployment. |

## Explore the Azure Resource Manager template parameters

In the parameters section of the template, you specify which values you can input when deploying the resources. The available properties for a parameter are:

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": "<minimum-value-for-int>",
        "maxValue": "<maximum-value-for-int>",
        "minLength": "<minimum-length-for-string-or-array>",
        "maxLength": "<maximum-length-for-string-or-array-parameters>",
        "metadata": {
            "description": "<description-of-the parameter>"
        }
    }
}
```

Here's an example that illustrates two parameters: one for a virtual machine's username, and one for its password:

```json
"parameters": {
  "adminUsername": {
    "type": "string",
    "metadata": {
      "description": "Username for the Virtual Machine."
    }
  },
  "adminPassword": {
    "type": "securestring",
    "metadata": {
      "description": "Password for the Virtual Machine."
    }
  }
}
```

> **Note:** you're limited to 256 parameters in a template. You can reduce the number of parameters by using objects that contain multiple properties.

## Consider Bicep templates

Azure Bicep is a domain-specific language (DSL) that uses declarative syntax to deploy Azure resources. It provides concise syntax, reliable type safety, and support for code reuse.

You can use Bicep instead of JSON to develop your Azure Resource Manager templates (ARM templates).

When you deploy a resource or series of resources to Azure, you submit the Bicep template to Resource Manager, which still requires JSON templates. The tooling that's built into Bicep transpiles, i.e., converts your Bicep template into a JSON template.

Bicep provides many improvements over JSON for template authoring, including:

- **Simpler syntax:** string interpolation, references using symbolic names, etc.
- **Modules:** you can break down complex template deployments into smaller module files and reference them in a main template.
- **Automatic dependency management:** in most situations, Bicep automatically detects dependencies between your resources.
- **Type validation and IntelliSense:** the Bicep extension for Visual Studio Code features rich validation and IntelliSense for all Azure resource type API definitions.

# Automate Azure tasks using scripts with PowerShell

Creating administration scripts is a powerfull way to optimize your work flow. You can automate common, repetitive tasks. Once a script has been verified, it will run consistently, likely reducing errors.

## Decide if Azure PowerShell is right for your tasks

Azure provides three administration tools:

- Azure portal
- Azure CLI
- Azure PowerShell

They all offer approximately the same amount of control; any task that you can do with one of the tools, you can likely do with the orther two. All three are cross-platform, running on Windows, macOS, and Linux. They differ in syntax, setup requirements, automation support.

### How to Choose an administrative tool

There's approximate parity between the portal, the Azure CLI, and Azure PowerShell with respect to the Azure objects they can administer and the configurations they can create. They're also all cross-platform. Typically, you'll consider several other factors when making your choice:

- **Automation:** Azure PowerShell and Azure CLI support automation, while Azure portal doesn't.
- **Learning curve:** The Azure portal doesn't require you to learn syntax or memorize commands. In Azure PowerShell and the Azure CLI, you must know the detailed syntax for each command you use.
- **Team skillset:** Your team may have used PowerShell to administer Windows. If so, they'll quickly become comfortable using Azure PowerShell.

## Create an Azure Resource using scripts in Azure PowerShell

Beginning with PowerShell 3.0, modules are loaded automatically when you use a cmdlet within the module. It's no longer necessary to manually import PowerShell modules unless you've changed the default module autoloading settings.

When you're working with a local install of Azure PowerShell, <u>you need to authenticate before you can execute Azure commands</u>. The `Connect-AzAccount` cmdlet prompts for your Azure credentials, then connects to your Azure subscription. It has many optional parameters, but if all you need is an interactive prompt, you don't need any parameters.

Use `Get-AzSubscription` to <u>obtain a list of all subscription names in your account</u>.

You can <u>change the subscription</u> by passing the name of the one to select.

```PowerShell
Set-AzContext -Subscription '00000000-0000-0000-0000-000000000000'
```

You can also <u>determine which subscription is active</u> by running `Get-AzContext`.

Use `Get-AzResourceGroup` to <u>retrieve a list of all Resource Groups in the active subscription</u>.

To get a more concise view, you can send the output from the `Get-AzResourceGroup` to the `Format-Table` cmdlet using a pipe '|':

```PowerShell
Get-AzResourceGroup | Format-Table
```

You can <u>create resource groups</u> by using the `New-AzResourceGroup` cmdlet. You must specify a name and location.

As with most of the Azure cmdlets, `New-AzResourceGroup` has many optional parameters. However, the core syntax is:

```PowerShell
New-AzResourceGroup -Name <name> -Location <location>
```

### Create an Azure Virtual Machine

Another common task you can do with PowerShell is to create VMs.

Azure PowerShell provides the `New-AzVm` cmdlet to create a virtual machine. The cmdlet has many parameters to let it handle the large number of VM configuration settings. Most of the parameters have reasonable default values, so we only need to specify five things:

- **ResourceGroupName:** The resource group into which the new VM should be placed.
- **Name:** The name of the VM in Azure.
- **Location:** Geographic location where the VM should be provisioned.
- **Credential:** An object containing the username and password for the VM admin account. We use the Get-Credential cmdlet. This cmdlet prompts for a username and password and packages it into a credential object.
- **Image:** The operating system image to use for the VM, which is typically a Linux distribution or Windows Server.

```PowerShell
New-AzVm
  -ResourceGroupName <resource group name>
  -Name <machine name>
  -Credential <credentials object>
  -Location <location>
  -Image <image name>
```
You can supply these parameters directly to the cmdlet as shown in the previous example. Alternatively, you can use other cmdlets to configure the virtual machine, such as `Set-AzVMOperatingSystem`, `Set-AzVMSourceImage`, `Add-AzVMNetworkInterface`, and `Set-AzVMOSDisk`.

Here's an example that strings the `Get-Credential` cmdlet together with the `-Credential` parameter:

```PowerShell
New-AzVM -Name MyVm -ResourceGroupName ExerciseResources -Credential (Get-Credential) ...
```

The `AzVM` suffix is specific to VM-based commands in PowerShell. THere are several others you can use:

| Command | Description |
| ------- | ----------- |
| `Remove-AzVM` | Deletes an Azure VM |
| `Start-AzVM` | Start a stopped VM |
| `Stop-AzVM` | Stop a running VM |
| `Restart-AzVM` | Restart a VM |
| `Update-AzVM` | Updates the configuration for a VM |

You can list the VMs in your subscription using the `Get-AzVM -Status` command. This command also supports entering a specific VM by including the `-Name` property. Here, we assign it to a PowerShell variable:

```PowerShell
$vm = Get-AzVM  -Name MyVM -ResourceGroupName ExerciseResources
```

The interesting thing is that now your VM is an object with which you can interact. For example, you can make changes to that object, then push changes back to Azure by using the `Update-AzVM` command:

```PowerShell
$ResourceGroupName = "ExerciseResources"
$vm = Get-AzVM  -Name MyVM -ResourceGroupName $ResourceGroupName
$vm.HardwareProfile.vmSize = "Standard_DS3_v2"

Update-AzVM -ResourceGroupName $ResourceGroupName  -VM $vm
```

The interactive mode in PowerShell is appropriate for one-off tasks. In our example, we use the same resource group for the lifetime of the project, so creating it interactively is reasonable. Interactive mode is often quicker and easier for this task than writing a script and executing that script exactly once.

