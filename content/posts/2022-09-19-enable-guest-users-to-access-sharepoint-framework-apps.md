---
title: Enable guest users to access SharePoint Framework apps
description: ""
date: 2022-09-19T17:08:33.057Z
preview: ""
draft: false
tags:
  - SharePoint
  - SPFx
  - PowerShell
categories:
  - Microsoft 365
  - SharePoint
---

Sharing your custom SharePoint Framework (SPFx) apps with guest users might cause some problems if you tenant is not set up for this.

Everything can work fine for you, but as a guest users error messages like **Something went wrong If the problem persists, contact the site administrator and give them the information in Technical Details.** appear instead of seeing your app. If they expand the error message it also contains information like **Original error: Failed to load path dependency "CommonStrings" from component**. This problem will occur to both WebParts and Extensions.

![](/assets/2022/09/sorry-something-went-wrong.png)

## Solution 1: Minimal path to success

The easies way to enable your guest users to access your SPFx apps in SharePoint Online is to enable the Public CDN. The Public CDN will by default include the **ClientSideAssets** library where all the assets from your apps are deployed and hosted in the App Catalog.

> NOTE: Enabling the public CDN should not be done without being aware what it actually does. A public CDN will result in the covered folders (by default includes /masterpage, /style library and /clientsideassets) to be anonymously accessible on the internet. There is a huge gap between the tight permissions that we are used to in Microsoft 365 in one end, and suddenly potentially exposing data on the internet with this feature. Microsoft has a clear disclaimer that no sensitive information must be added to a public CDN source. But how can be ensure this? You must in fact review the content of all apps in the App Catalog contains today, and for every deploy in the future. Having a mix of 3. party and custom apps makes this even more complex. So from my point of view, I would avoid enabling this unless required to be better safe than sorry. Reference: https://learn.microsoft.com/en-us/microsoft-365/enterprise/use-microsoft-365-cdn-with-spo?view=o365-worldwide#CDNOriginChoosePublicPrivate

As an SharePoint administrator you can enable this using the PnP.PowerShell module:

```powershell
Connect-PnPOnline -Url https://TENANTNAME-admin.sharepoint.com/
Set-PnPTenantCdnEnabled -CdnType Public -Enable $true
```

## Solution 2: Give guest users access to only the necessary files in the App Catalog

My recommended way to enable guest access to the App Catalog will be to give the minimum needed permissions. At this time it is only necessary to give external users read access to the **ClientSideAssets** library. 

1. First we need to temporary enable the **Everyone* claim that included permissions for both internal and external users.

```powershell
Connect-PnPOnline -Url https://TENANTNAME-admin.sharepoint.com/
Set-PnPTenant -ShowEveryoneClaim $true
```

2. Then we can go to the app catalog: https://TENANTID-admin.sharepoint.com/_layouts/15/tenantAppCatalog.aspx

3. In the URL of the site your are redirected to, replace **_layouts/15/tenantAppCatalog.aspx/manageApps** with **ClientSideAssets**
4. Select **Library** and **Library settings**
5. Select **Permissions for this document library** and **Stop inheriting permissions**, confirm with **Ok**
6. Select **Grant permissions** and fill of the form like this and add the permissions with **Share**

![](/assets/2022/09/share-clientsideassets.png)

The permissions to the **ClientSideAssets** library should now at least contains these:
![](/assets/2022/09/permissions-clientsideassets.png)

7. Go to the SharePoint admin center: https://TENANTNAME-admin.sharepoint.com/
8. Select **Sites**, **Active sites** and search for your App Catalog:

![](/assets/2022/09/app-catalog.png)

9. Select the site and locate the *Polices* tab and check the current sharing policy:

![](/assets/2022/09/app-catalog-sharing.png)

10. This policy should be **Existing users only**, if not select **Edit** and change it:

![](/assets/2022/09/app-catalog-sharing-change.png)

11. The final step is to hide the **Everyone** claim that we enabled in step 1:

```powershell
Connect-PnPOnline -Url https://TENANTNAME-admin.sharepoint.com/
Set-PnPTenant -ShowEveryoneClaim $false
```

Now it should be possible for both internal and external users that have access to a site in your tenant to also see the SPFx added to the sites like in this example with Modern Search:

![](/assets/2022/09/spfx-shared-with-guests.png)

> NOTE: If your guest users has tried to access the sites prior to completing this setup, they might not immediately see any change. This will require a clearing of the browser cache to ensure that the assets are reloaded properly.

## Summary

Enabling guests access to custom apps deployed in the SharePoint Framework can be complex, and many different settings in your tenant like CDN, sharing policy, conditional access, etc can cause problem. 