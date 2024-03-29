---
description: Provider types
ms.date: 08/21/2019
ms.topic: reference
title: Provider types
---

# Provider types

Providers define their basic functionality by changing how the provider cmdlets, provided by
PowerShell, perform their actions. For example, providers can use the default functionality of the
`Get-Item` cmdlet, or they can change how that cmdlet operates when retrieving items from the data
store. The provider functionality described in this document includes functionality defined by
overwriting methods from specific provider base classes and interfaces.

> [!NOTE]
> For provider features that are pre-defined by PowerShell, see [Provider capabilities](/previous-versions//ee126189(v=vs.85)).

## Drive-enabled providers

Drive-enabled providers specify the default drives available to the user and allow the user to add
or remove drives. In most cases, providers are drive-enabled providers because they require some
default drive to access the data store. However, when writing your own provider you might or might
not want to allow the user to create and remove drives.

To create a drive-enabled provider, your provider class must derive from the
[System.Management.Automation.Provider.DriveCmdletProvider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider)
class or another class that derives from that class. The
**DriveCmdletProvider** class defines the following methods
for implementing the default drives of the provider and supporting the `New-PSDrive` and
`Remove-PSDrive` cmdlets. In most cases, to support a provider cmdlet you must overwrite the method
that the PowerShell engine calls to invoke the cmdlet, such as the `NewDrive` method for the
`New-PSDrive` cmdlet, and optionally you can overwrite a second method, such as
`NewDriveDynamicParameters`, for adding dynamic parameters to the cmdlet.

- The
  [InitializeDefaultDrives](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.InitializeDefaultDrives)
  method defines the default drives that are available to the user whenever the provider is used.

- The
  [NewDrive](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.NewDrive)
  and
  [NewDriveDynamicParameters](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.NewDriveDynamicParameters)
  methods defines how your provider supports the `New-PSDrive` provider cmdlet. This cmdlet allows
  the user to create drives to access the data store.

- The
  [RemoveDrive](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.RemoveDrive)
  method defines how your provider supports the `Remove-PSDrive` provider cmdlet. This cmdlet allows
  the user to remove drives from the data store.

## Item-enabled providers

Item-enabled providers allow the user to get, set, or clear the items in the data store. An "item"
is an element of the data store that the user can access or manage independently. To create an
item-enabled provider, your provider class must derive from the
[System.Management.Automation.Provider.ItemCmdletProvider](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider)
class or another class that derives from that class.

The **ItemCmdletProvider** class defines the following methods
for implementing specific provider cmdlets. In most cases, to support a provider cmdlet you must
overwrite the method that the PowerShell engine calls to invoke the cmdlet, such as the `ClearItem`
method for the `Clear-Item` cmdlet, and optionally you can overwrite a second method, such as
`ClearItemDynamicParameters`, for adding dynamic parameters to the cmdlet.

- The
  [ClearItem](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ClearItem)
  and
  [ClearItemDynamicParameters](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ClearItemDynamicParameters)
  methods define how your provider supports the `Clear-Item` provider cmdlet. This cmdlet allows the
  user to remove of the value of an item in the data store.

- The
  [GetItem](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItem)
  and
  [GetItemDynamicParameters](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItemDynamicParameters)
  methods define how your provider supports the `Get-Item` provider cmdlet. This cmdlet allows the
  user to retrieve data from the data store.

- The
  [SetItem](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem)
  and
  [SetItemDynamicParameters](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItemDynamicParameters)
  methods define how your provider supports the `Set-Item` provider cmdlet. This cmdlet allows the
  user to update the values of items in the data store.

- The
  [InvokeDefaultAction](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.InvokeDefaultAction)
  and
  [InvokeDefaultActionDynamicParameters](/dotnet/api/system.management.automation.provider.itemcmdletprovider.invokedefaultactiondynamicparameters)
  methods define how your provider supports the `Invoke-Item` provider cmdlet. This cmdlet allows
  the user to perform the default action specified by the item.

- The
  [ItemExists](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExists)
  and
  [ItemExistsDynamicParameters](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExistsDynamicParameters)
  methods define how your provider supports the `Test-Path` provider cmdlet. This cmdlet allows the
  user to determine if all the elements of a path exist.

In addition to the methods used to implement provider cmdlets, the
**ItemCmdletProvider** class also defines the following
methods:

- The
  [ExpandPath](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ExpandPath)
  method allows the user to use wildcards when specifying the provider path.

- The
  [IsValidPath](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.IsValidPath)
  is used to determine if a path is syntactically and semantically valid for the provider.

## Container-enabled providers

Container-enabled providers allow the user to manage items that are containers. A container is a
group of child items under a common parent item. To create a container-enabled provider, your
provider class must derive from the
[System.Management.Automation.Provider.ContainerCmdletProvider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider)
class or another class that derives from that class.

> [!IMPORTANT]
> Container-enabled providers can't access data stores that contain nested containers. If a child
> item of a container is another container, you must implement a navigation-enabled provider.

The **ContainerCmdletProvider** class defines the following
methods for implementing specific provider cmdlets. In most cases, to support a provider cmdlet you
must overwrite the method that the PowerShell engine calls to invoke the cmdlet, such as the
`CopyItem` method for the `Copy-Item` cmdlet, and optionally you can overwrite a second method, such
as `CopyItemDynamicParameters`, for adding dynamic parameters to the cmdlet.

- The
  [CopyItem](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItem)
  and
  [CopyItemDynamicParameters](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItemDynamicParameters)
  methods define how your provider supports the `Copy-Item` provider cmdlet. This cmdlet allows the
  user to copy an item from one location to another.

- The
  [GetChildItems](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems)
  and
  [GetChildItemsDynamicParameters](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItemsDynamicParameters)
  methods define how your provider supports the `Get-ChildItem` provider cmdlet. This cmdlet allows
  the user to retrieve the child items of the parent item.

- The
  [GetChildNames](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildNames)
  and
  [GetChildNamesDynamicParameters](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildNamesDynamicParameters)
  methods define how your provider supports the `Get-ChildItem` provider cmdlet if its `Name`
  parameter is specified.

- The
  [NewItem](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem)
  and
  [NewItemDynamicParameters](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItemDynamicParameters)
  methods define how your provider supports the `New-Item` provider cmdlet. This cmdlet allows the
  user to create new items in the data store.

- The
  [RemoveItem](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItem)
  and
  [RemoveItemDynamicParameters](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItemDynamicParameters)
  methods define how your provider supports the `Remove-Item` provider cmdlet. This cmdlet allows
  the user to remove items from the data store.

- The
  [RenameItem](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItem)
  and
  [RenameItemDynamicParameters](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItemDynamicParameters)
  methods define how your provider supports the `Rename-Item` provider cmdlet. This cmdlet allows
  the user to rename items in the data store.

In addition to the methods used to implement provider cmdlets, the
**ContainerCmdletProvider** class also defines the following
methods:

- The
  [HasChildItems](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.HasChildItems)
  method can be used by the provider class to determine whether an item has child items.

- The
  [ConvertPath](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.ConvertPath)
  method can be used by the provider class to create a new provider-specific path from a specified
  path.

## Navigation-enabled providers

Navigation-enabled providers allow the user to move items in the data store. To create a
navigation-enabled provider, your provider class must derive from the
[System.Management.Automation.Provider.NavigationCmdletProvider](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider)
class.

The **NavigationCmdletProvider** class defines the following
methods for implementing specific provider cmdlets. In most cases, to support a provider cmdlet you
must overwrite the method that the PowerShell engine calls to invoke the cmdlet, such as the
`MoveItem` method for the `Move-Item` cmdlet, and optionally you can overwrite a second method, such
as `MoveItemDynamicParameters`, for adding dynamic parameters to the cmdlet.

- The
  [MoveItem](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItem)
  and
  [MoveItemDynamicParameters](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItemDynamicParameters)
  methods define how your provider supports the `Move-Item` provider cmdlet. This cmdlet allows the
  user to move an item from one location in the store to another location.

- The
  [MakePath](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MakePath)
  method defines how your provider supports the `Join-Path` provider cmdlet. This cmdlet allows the
  user to combine a parent and child path segment to create a provider-internal path.

In addition to the methods used to implement provider cmdlets, the
**NavigationCmdletProvider** class also defines the following
methods:

- The
  [GetChildName](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.GetChildName)
  method extracts the name of the child node of a path.

- The
  [GetParentPath](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.GetParentPath)
  method extracts the parent part of a path.

- The
  [IsItemContainer](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.IsItemContainer)
  method determines whether the item is a container item. In this context, a container is a group of
  child items under a common parent item.

- The
  [NormalizeRelativePath](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.NormalizeRelativePath)
  method returns a path to an item that's relative to a specified base path.

## Content-enabled providers

Content-enabled providers allow the user to clear, get, or set the content of items in a data store.
For example, the FileSystem provider allows you to clear, get, and set the content of files in the
file system. To create a content enabled provider, your provider class must implement the methods of
the
[System.Management.Automation.Provider.IContentCmdletProvider](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider)
interface.

The **IContentCmdletProvider** interface defines the following
methods for implementing specific provider cmdlets. In most cases, to support a provider cmdlet you
must overwrite the method that the PowerShell engine calls to invoke the cmdlet, such as the
`ClearContent` method for the `Clear-Content` cmdlet, and optionally you can overwrite a second
method, such as `ClearContentDynamicParameters`, for adding dynamic parameters to the cmdlet.

- The
  [ClearContent](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.ClearContent)
  and
  [ClearContentDynamicParameters](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.ClearContentDynamicParameters)
  methods define how your provider supports the `Clear-Content` provider cmdlet. This cmdlet allows
  the user to delete the content of an item without deleting the item.

- The
  [GetContentReader](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentReader)
  and
  [GetContentReaderDynamicParameters](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentReaderDynamicParameters)
  methods define how your provider supports the `Get-Content` provider cmdlet. This cmdlet allows
  the user to retrieve the content of an item. The
  `GetContentReader` method returns an
  [System.Management.Automation.Provider.IContentReader](/dotnet/api/System.Management.Automation.Provider.IContentReader)
  interface that defines the methods used to read the content.

- The
  [GetContentWriter](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentWriter)
  and
  [GetContentWriterDynamicParameters](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentWriterDynamicParameters)
  methods define how your provider supports the `Set-Content` provider cmdlet. This cmdlet allows
  the user to update the content of an item. The
  `GetContentWriter` method returns an
  [System.Management.Automation.Provider.IContentWriter](/dotnet/api/System.Management.Automation.Provider.IContentWriter)
  interface that defines the methods used to write the content.

## Property-enabled providers

Property-enabled providers allow the user to manage the properties of the items in the data store.
To create a property-enabled provider, your provider class must implement the methods of the
[System.Management.Automation.Provider.IPropertyCmdletProvider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider)
and
[System.Management.Automation.Provider.IDynamicPropertyCmdletProvider](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider)
interfaces. In most cases, to support a provider cmdlet you must overwrite the method that the
PowerShell engine calls to invoke the cmdlet, such as the `ClearProperty` method for the
Clear-Property cmdlet, and optionally you can overwrite a second method, such as
`ClearPropertyDynamicParameters`, for adding dynamic parameters to the cmdlet.

The **IPropertyCmdletProvider** interface defines the
following methods for implementing specific provider cmdlets:

- The
  [ClearProperty](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearProperty)
  and
  [ClearPropertyDynamicParameters](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearPropertyDynamicParameters)
  methods define how your provider supports the `Clear-ItemProperty` provider cmdlet. This cmdlet
  allows the user to delete the value of a property.

- The
  [GetProperty](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetProperty)
  and
  [GetPropertyDynamicParameters](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetPropertyDynamicParameters)
  methods define how your provider supports the `Get-ItemProperty` provider cmdlet. This cmdlet
  allows the user to retrieve the property of an item.

- The
  [SetProperty](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetProperty)
  and
  [SetPropertyDynamicParameters](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetPropertyDynamicParameters)
  methods define how your provider supports the `Set-ItemProperty` provider cmdlet. This cmdlet
  allows the user to update the properties of an item.

The **IDynamicPropertyCmdletProvider** interface defines the
following methods for implementing specific provider cmdlets:

- The
  [CopyProperty](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.CopyProperty)
  and
  [CopyPropertyDynamicParameters](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.CopyPropertyDynamicParameters)
  methods define how your provider supports the `Copy-ItemProperty` provider cmdlet. This cmdlet
  allows the user to copy a property and its value from one location to another.

- The
  [MoveProperty](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.MoveProperty)
  and
  [MovePropertyDynamicParameters](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.MovePropertyDynamicParameters)
  methods define how your provider supports the `Move-ItemProperty` provider cmdlet. This cmdlet
  allows the user to move a property and its value from one location to another.

- The
  [NewProperty](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.NewProperty)
  and
  [NewPropertyDynamicParameters](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.NewPropertyDynamicParameters)
  methods define how your provider supports the `New-ItemProperty` provider cmdlet. This cmdlet
  allows the user to create a new property and set its value.

- The
  [RemoveProperty](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.RemoveProperty)
  and
  [RemovePropertyDynamicParameters](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.RemovePropertyDynamicParameters)
  methods define how your provider supports the `Remove-ItemProperty` cmdlet. This cmdlet allows the
  user to delete a property and its value.

- The
  [RenameProperty](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.RenameProperty)
  and
  [RenamePropertyDynamicParameters](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.RenamePropertyDynamicParameters)
  methods define how your provider supports the `Rename-ItemProperty` cmdlet. This cmdlet allows the
  user to change the name of a property.

## See also

[about_Providers](/powershell/module/microsoft.powershell.core/about/about_providers)

[Writing a Windows PowerShell Provider](./writing-a-windows-powershell-provider.md)
