---
title: 'Tutorial: Configure Humbol for automatic user provisioning with Azure Active Directory'
description: Learn how to automatically provision and de-provision user accounts from Azure AD to Humbol.
services: active-directory
author: twimmers
writer: twimmers
manager: jeedes
ms.assetid: a34b8778-3a56-4a39-835d-54044079350d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/02/2023
ms.author: thwimmer
---

# Tutorial: Configure Humbol for automatic user provisioning

This tutorial describes the steps you need to perform in both Humbol and Azure Active Directory (Azure AD) to configure automatic user provisioning. When configured, Azure AD automatically provisions and de-provisions users to [Humbol](https://www.humbol.app/en/product/) using the Azure AD Provisioning service. For important details on what this service does, how it works, and frequently asked questions, see [Automate user provisioning and deprovisioning to SaaS applications with Azure Active Directory](../app-provisioning/user-provisioning.md). 


## Supported capabilities
> [!div class="checklist"]
> * Create users in Humbol.
> * Remove users in Humbol when they do not require access anymore.
> * Keep user attributes synchronized between Azure AD and Humbol.
> * [Single sign-on](../manage-apps/add-application-portal-setup-oidc-sso.md) to Humbol (recommended).

## Prerequisites

The scenario outlined in this tutorial assumes that you already have the following prerequisites:

* [An Azure AD tenant](../develop/quickstart-create-new-tenant.md). 
* A user account in Azure AD with [permission](../roles/permissions-reference.md) to configure provisioning (for example, Application Administrator, Cloud Application administrator, Application Owner, or Global Administrator).
* Active contract with Humbol including SCIM API usage with Humbol Inc.
* A user account in Humbol with Admin permissions.

## Step 1. Plan your provisioning deployment
1. Learn about [how the provisioning service works](../app-provisioning/user-provisioning.md).
1. Determine who will be in [scope for provisioning](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
1. Determine what data to [map between Azure AD and Humbol](../app-provisioning/customize-application-attributes.md).

## Step 2. Configure Humbol to support provisioning with Azure AD
Contact Humbol support to configure Humbol to support provisioning with Azure AD.

1. As Humbol Admin login to your [Humbol](https://my.humbol.app/login) organization.
1. Go to organization's API [settings page](https://my.humbol.app/settings#apis).
   1. On this page you can find the organization SCIM API url. Copy it.
   1. Create SCIM API token and copy the value. 
   > [!NOTE]
   > The token value is not saved anywhere on the Humbol service, so if you lose it, you should create a new one and remove old one.

## Step 3. Add Humbol from the Azure AD application gallery

Add Humbol from the Azure AD application gallery to start managing provisioning to Humbol. If you have previously setup Humbol for SSO you can use the same application. However it's recommended that you create a separate app when testing out the integration initially. Learn more about adding an application from the gallery [here](../manage-apps/add-application-portal.md). 

## Step 4. Define who will be in scope for provisioning 

The Azure AD provisioning service allows you to scope who will be provisioned based on assignment to the application and or based on attributes of the user. If you choose to scope who will be provisioned to your app based on assignment, you can use the following [steps](../manage-apps/assign-user-or-group-access-portal.md) to assign users to the application. If you choose to scope who will be provisioned based solely on attributes of the user, you can use a scoping filter as described [here](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* Start small. Test with a small set of users before rolling out to everyone. When scope for provisioning is set to assigned users, you can control this by assigning one or two users to the app. When scope is set to all users, you can specify an [attribute based scoping filter](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

* If you need more roles, you can [update the application manifest](../develop/howto-add-app-roles-in-azure-ad-apps.md) to add new roles.


## Step 5. Configure automatic user provisioning to Humbol 

This section guides you through the steps to configure the Azure AD provisioning service to create, update, and disable users in TestApp based on user assignments in Azure AD.

### To configure automatic user provisioning for Humbol in Azure AD:

1. Sign in to the [Azure portal](https://portal.azure.com). Select **Enterprise Applications**, then select **All applications**.

	![Screenshot of Enterprise applications blade.](common/enterprise-applications.png)

1. In the applications list, select **Humbol**.

	![Screenshot of the Humbol link in the Applications list.](common/all-applications.png)

1. Select the **Provisioning** tab.

	![Screenshot of Provisioning tab.](common/provisioning.png)

1. Set the **Provisioning Mode** to **Automatic**.

	![Screenshot of Provisioning tab automatic.](common/provisioning-automatic.png)

1. Under the **Admin Credentials** section, input your Humbol Tenant URL and Secret Token. Click **Test Connection** to ensure Azure AD can connect to Humbol. If the connection fails, ensure your Humbol account has Admin permissions and try again.

 	![Screenshot of Token.](common/provisioning-testconnection-tenanturltoken.png)

1. In the **Notification Email** field, enter the email address of a person who should receive the provisioning error notifications and select the **Send an email notification when a failure occurs** check box.

	![Screenshot of Notification Email.](common/provisioning-notification-email.png)

1. Select **Save**.

1. Under the **Mappings** section, select **Synchronize Azure Active Directory Users to Humbol**.

1. Review the user attributes that are synchronized from Azure AD to Humbol in the **Attribute-Mapping** section. The attributes selected as **Matching** properties are used to match the user accounts in Humbol for update operations. If you choose to change the [matching target attribute](../app-provisioning/customize-application-attributes.md), you'll need to ensure that the Humbol API supports filtering users based on that attribute. Select the **Save** button to commit any changes.

   |Attribute|Type|Supported for filtering|Required by Humbol|
   |---|---|---|---|
   |userName|String|&check;|&check;
   |active|Boolean||&check;
   |title|String||
   |emails[type eq "work"].value|String||&check;
   |preferredLanguage|String||
   |name.givenName|String||&check;
   |name.familyName|String||&check;
   |addresses[type eq "work"].locality|String||
   |addresses[type eq "work"].region|String||
   |addresses[type eq "work"].country|String||
   |roles[primary eq "True"].value|String||
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|String||
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|String||

   > [!NOTE]
   > * If you include `roles[primary eq "True"].value` every user must have precisely one role. 
   > * Another option is to remove the role attribute mapping and manage Humbol user roles inside the Humbol application.
   
1. To configure scoping filters, refer to the following instructions provided in the [Scoping filter tutorial](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

1. To enable the Azure AD provisioning service for Humbol, change the **Provisioning Status** to **On** in the **Settings** section.

	![Screenshot of Provisioning Status Toggled On.](common/provisioning-toggle-on.png)

1. Define the users that you would like to provision to Humbol by choosing the desired values in **Scope** in the **Settings** section.

	![Screenshot of Provisioning Scope.](common/provisioning-scope.png)

1. When you're ready to provision, click **Save**.

	![Screenshot of Saving Provisioning Configuration.](common/provisioning-configuration-save.png)

This operation starts the initial synchronization cycle of all users defined in **Scope** in the **Settings** section. The initial cycle takes longer to perform than subsequent cycles, which occur approximately every 40 minutes as long as the Azure AD provisioning service is running. 

## Step 6. Monitor your deployment
Once you've configured provisioning, use the following resources to monitor your deployment:

* Use the [provisioning logs](../reports-monitoring/concept-provisioning-logs.md) to determine which users have been provisioned successfully or unsuccessfully
* Check the [progress bar](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) to see the status of the provisioning cycle and how close it's to completion
* If the provisioning configuration seems to be in an unhealthy state, the application goes into quarantine. Learn more about quarantine states [here](../app-provisioning/application-provisioning-quarantine-status.md).

## More resources

* [Managing user account provisioning for Enterprise Apps](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## Next steps

* [Learn how to review logs and get reports on provisioning activity](../app-provisioning/check-status-user-account-provisioning.md)
