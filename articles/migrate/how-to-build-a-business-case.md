---
title: Build a Business case with Azure Migrate | Microsoft Docs
description: Describes how to build a Business case with Azure Migrate
ms.service: azure-migrate
ms.topic: how-to
ms.date: 03/20/2025
ms.custom: engagement-fy23

# Customer intent: "As a cloud architect, I want to build a business case using Azure Migrate, so that I can assess the cost and benefits of migrating on-premises workloads to Azure for effective decision-making."
---

# Build a business case (preview)

This article describes how to build a Business case for on-premises servers and workloads in your datacenter with Azure Migrate: Discovery and assessment tool.

[Azure Migrate](migrate-services-overview.md) helps you to plan and execute migration and modernization projects to Azure. Azure Migrate provides a centralized hub to track discovery, assessment, and migration of on-premises infrastructure, applications, and data to Azure. The hub provides Azure tools for assessment and migration, and third-party independent software vendor (ISV) offerings.

## Prerequisites

- Make sure you've [created](./create-manage-projects.md) an Azure Migrate project. You can also reuse an existing project to use this capability.
::: moniker range="migrate-classic"
- Once you've created a project, the Azure Migrate: Discovery and assessment tool is automatically added to the project.
::: moniker-end
- Before you build the Business case, you need to first discover your IT estate. You can choose one of the two [discovery sources](./concepts-business-case-calculation.md#discovery-sources-to-create-a-business-case) based on your use case.


## Business case overview

The Business case capability helps you build a business proposal to understand how Azure can bring the most value to your business. It highlights:

- On-premises vs Azure total cost of ownership.
- Year on year cashflow analysis.
- Resource utilization based insights to identify servers and workloads that are ideal for cloud.
- Quick wins for migration and modernization including end of support Windows OS and SQL versions.
- Long term cost savings by moving from a capital expenditure model to an Operating expenditure model, by paying for only what you use.
- Sustainability insights to indicate a reduction in carbon emissions when migrating from on-premises to Azure.
- (Optional) Current on-premises vs On-premises with Arc total cost of ownership. 
- (Optional) the cost savings and other benefits of using Azure security (Microsoft Defender for Cloud) and management (Azure Monitor and Update Management) via Arc, and ESUs enabled by Arc for your on-premises servers.

Other key features:

- Helps remove guess work in your cost planning process and adds data insights driven calculations.
- It can be generated in just a few selections after you have performed discovery using the Azure Migrate appliance.
- The feature is automatically enabled for existing Azure Migrate projects.

There are three types of migration strategies that you can choose while building your Business case:

**Migration Strategy** | **Details** | **Assessment insights**
--- | --- | ---
**Modernize** | You can receive a PaaS recommendation, which means the logic identifies workloads that are best suited for PaaS targets. <br/> <br/> It is recommended to use general servers with a straightforward lift-and-shift approach to Azure IaaS.|  For SQL Servers, sizing and cost comes from the Instance to Azure SQL MI report.<br/> For web apps, sizing and cost comes from Azure App Service and Azure Kubernetes Service assessments, with a preference to App Service. <br/> For general servers, sizing and cost comes from Azure VM assessment.
**Minimize Migration time with Azure VM** | You can quickly get a recommendation to lift and shift to an Azure VM. | For SQL Servers, sizing and cost comes from the Instance to SQL Server on Azure VM report.<br/><br/> For general servers and servers hosting web apps, sizing and cost comes from Azure VM assessment.
**Minimize Migration time with AVS** | You can quickly get a recommendation to migrate to Azure VMware Solution (AVS).| For all servers, sizing and cost are determined by an Azure VMware Solution assessment.

> [!Note]
> Although the Business case picks Azure recommendations from certain assessments, you won't be able to access the assessments directly. To deep dive into sizing, readiness and Azure cost estimates, you can create respective assessments for the servers or workloads.

## Build a business case

To build a business case, follow these steps:

::: moniker range="migrate"
1. Select your project from the **All Projects**. 

    :::image type="content" source="./media/how-to-build-a-business-case/all-projects.png" alt-text="Screenshot on how to build a project.":::

1. In the overview page, select **Build business case**.

    :::image type="content" source="./media/how-to-build-a-business-case/build-a-business-case.png" alt-text="Screenshot of the Build Business case.":::

    > [!Note]
    > We recommend waiting at least a day after starting discovery before building a Business case. This period allows for adequate performance and resource utilization data collection. Also, review the **Action Centre** blade on the Azure Migrate overview page to identify any discovery-related issues. This ensures a more accurate representation of your datacenter's IT estate, making the Business case recommendations more valuable.

1. In **Business case name**, specify a name for the Business case. Make sure the Business case name is unique within a project, else the previous Business case with the same name gets recomputed.
1. In **Discovery source**, specify the discovery source on which you wish to create the business case. The options to build the business case using data discovered via the  appliance or imported via a .csv file is present based on the type of discovered servers present in your project. The discovery source will be defaulted to the option chosen by you while building the business case and you won't be able to update this field later.

1. Select the Scope of business case. 
    1. Select **Entire datacenter** your business case is created for all discovered workloads.
    
    :::image type="content" source="./media/how-to-build-a-business-case/entire-data-center.png" alt-text="Screenshot shows how to select the entire data center.":::

1. Select the **Selected Workloads**, you can create a business case for a specific set of workloads.
1. Select **Add Workloads** and then select the workloads you want to include in your business case. You can filter and select based on the tags and criteria you’ve set.
1. Select **Next** to proceed.

    :::image type="content" source="./media/how-to-build-a-business-case/add-workloads.png" alt-text="Screenshot shows how to add workloads.":::

1. In **Target location**, specify the Azure region to which you want to migrate.

   Azure SKU size and cost recommendations are based on the location that you specify.

1. In **Migration preference**, specify the migration preference for your Business case:
    
    - With the default *Azure recommended approach to minimize cost*, you can get the most cost-efficient and compatible target recommendation in Azure across Azure IaaS and Azure PaaS targets.
    - With the Minimize Migration Time option, you can get a quick lift-and-shift recommendation to Azure IaaS offerings like Azure VM and Azure VMware Solution. 
    - Select **Preferred Target** as Azure VM for a quick lift and shift recommendation to Azure VM 
    - Select **Preferred Target** as Azure VMware Solution (AVS) for the most cost-effective and compatible target recommendation for hosting workloads on AVS.
    > [!Note]
    > Only Reserved Instances are available as a savings option for migrating to AVS.

1. Enter the Savings options. This specifies the combination that you want to consider while optimizing Azure costs to maximize savings.

    Based on the availability of the savings option in your selected region and the targets, the business case recommends the appropriate options to help you save the most on Azure.
    - Select **Reserved Instance** if your datacenter mainly consists of resources that run consistently.
    - Select **Reserved Instance** and **Azure Savings Plan** if you need more flexibility and automated cost optimization for workloads eligible under Azure Savings Plan (Compute targets including Azure VM and Azure App Service).

1. In **Discount (%) on Pay as you go**, add any subscription-specific discounts you receive on top of the Azure offer. The default setting is 0%. The discount isn't applicable on top of reserved instance savings option.
1. Select the currency in which you want to create the business case. 
1. Review your selected inputs, and then select **Build business case**.

    :::image type="content" source="./media/how-to-build-a-business-case/review-and-build.png" alt-text="Screenshot shows to review and build a business case.":::

1. You are directed to the newly created Business case with a banner indicating that the computation is in progress. The processing time might vary based on the number of servers and workloads involved. It is recommended to return to the Business case page after approximately 30 minutes and select the **Refresh**.
    
    :::image type="content" source="./media/how-to-build-a-business-case/refresh-build.png" alt-text="Screenshot of the refresh button to refresh the Business case.":::

::: moniker-end

::: moniker range="migrate-classic"

To build a business case, follow these steps:

1. In **Servers, databases and web apps**, select **Discover, assess and migrate**.

    :::image type="content" source="./media/how-to-build-a-business-case/assess-inline.png" alt-text="Screenshot of the discover, assess and migrate servers button." lightbox="./media/how-to-build-a-business-case/assess-expanded.png":::

1. In **Azure Migrate: Discovery and assessment**, select **Build business case**.

    :::image type="content" source="./media/how-to-build-a-business-case/build-inline.png" alt-text="Screenshot of the Build Business case button." lightbox="./media/how-to-build-a-business-case/build-expanded.png":::

    > [!Note]
    > We recommend you wait at least a day after you start discovery before you build a Business case. This allows enough collection of performance/resource utilization data points. Also, review the **Notifications**/**Resolve issues** blades on the Azure Migrate hub to identify any discovery related issues prior to Business case computation. It ensures that the IT estate in your datacenter is represented more accurately and the Business case recommendations are more valuable.

1. In **Business case name**, specify a name for the Business case. Make sure the Business case name is unique within a project, else the previous Business case with the same name gets recomputed.

1. In **Target location**, specify the Azure region to which you want to migrate.

   Azure SKU size and cost recommendations are based on the location that you specify.

1. In **Discovery source**, specify the discovery source on which you wish to create the business case. The options to build the business case using data discovered via the  appliance or imported via a .csv file is present based on the type of discovered servers present in your project. The discovery source will be defaulted to the option chosen by you while building the business case and you won't be able to update this field later.

1. In **Migration strategy**, specify the migration strategy for your Business case:
    
    - With the default *Azure recommended approach to minimize cost*, you can get the most cost-efficient and compatible target recommendation in Azure across Azure IaaS and Azure PaaS targets.
    - With *Migrate to all IaaS (Infrastructure as a Service)*, you can get a quick lift and shift recommendation to Azure IaaS.
    - With *Modernize to PaaS (Platform as a Service)*, you can get cost effective recommendations for Azure IaaS and more PaaS preferred targets in Azure PaaS.
    - With *Migrate to AVS (Azure VMware Solution)*, you can get the most cost effective and compatible target recommendation for hosting workloads on AVS. Only Reserved Instances are available as a savings option for migrating to AVS. 
1. In **Savings options**, specify the savings options combination that you want to be considered while optimizing your Azure costs and maximize savings. Based on the availability of the savings option in the chosen region and the targets, the business case recommends the appropriate savings options to maximize your savings on Azure.
    - Choose 'Reserved Instance', if your datacenter comprises most consistently running resources.
    - Choose 'Reserved Instance + Azure Savings Plan', if you want additional flexibility and automated cost optimization for workloads applicable for Azure Savings Plan (Compute targets including Azure VM and Azure App Service).

1. In **Discount (%) on Pay as you go**, add any subscription-specific discounts you receive on top of the Azure offer. The default setting is 0%. The discount isn't applicable on top of reserved instance savings option.
1. **Currency** is defaulted to USD and can't be edited.
1. Review the chosen inputs, and select **Build business case**.

    :::image type="content" source="./media/how-to-build-a-business-case/build-button.png" alt-text="Screenshot of the button to initiate the Business case creation.":::

1. You're directed to the newly created Business case with a banner that says that your Business case is computing. The computation might take some time, depending on the number of servers and workloads in the project. You can come back to the Business case page after ~30 minutes and select **Refresh**.
    
    :::image type="content" source="./media/how-to-build-a-business-case/refresh-inline.png" alt-text="Screenshot of the refresh button to refresh the Business case." lightbox="./media/how-to-build-a-business-case/refresh-expanded.png":::

::: moniker-end

## Next steps
- [Learn more](how-to-view-a-business-case.md) about how to review the Business case reports.