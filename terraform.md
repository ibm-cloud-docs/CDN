---

copyright:
  years: 2023
lastupdated: "2023-02-09"

subcollection: CDN

---

{{site.data.keyword.attribute-definition-list}}

# Setting up Terraform for CDN
{: #terraform-setup-cis}

Terraform on {{site.data.keyword.cloud}} enables predictable and consistent provisioning of {{site.data.keyword.cloud_notm}} services so that you can rapidly build complex, multi-tier cloud environments following Infrastructure as Code (IaC) principles. Similar to using the {{site.data.keyword.cloud_notm}} CLI or API and SDKs, you can automate the provisioning, update, and deletion of your {{site.data.keyword.cis_full_notm}} instances by using HashiCorp Configuration Language (HCL).
{: shortdesc}

Looking for a managed Terraform on {{site.data.keyword.cloud}} solution? Try out [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started). With {{site.data.keyword.bpshort}}, you can use the Terraform scripting language that you are familiar with, but you don't have to worry about setting up and maintaining the Terraform command line and the {{site.data.keyword.cloud}} Provider plug-in. {{site.data.keyword.bpshort}} also provides pre-defined Terraform templates that you can easily install from the {{site.data.keyword.cloud}} catalog.
{: tip}

## Installing Terraform and configuring resources for CDN
{: #install-terraform}

Before you begin, make sure that you have the [required access](/docs/account?topic=account-mngclassicinfra) to create and work with Content Delivery Network (CDN) resources.

- Follow the [Terraform on {{site.data.keyword.cloud}} getting started tutorial](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started) to install the Terraform CLI and configure the {{site.data.keyword.cloud}} Provider plug-in for Terraform. The plug-in abstracts the {{site.data.keyword.cloud}} APIs that are used to provision, update, or delete {{site.data.keyword.cis_full_notm}} service instances and resources.
- Create a Terraform configuration file that is named `main.tf`. In this file, you add the configuration to create a {{site.data.keyword.cis_short_notm}} service instance and to assign a user an access policy in Identity and Access Management (IAM) for that instance by using HashiCorp Configuration Language (HCL). For more information, see the [Terraform documentation](https://www.terraform.io/docs/language/index.html){: external}.

1. Create a CDN instance by using the `ibm_resource_instance` resource argument in your `main.tf` file.
   The CDN instance in the following example is named `test_cdn1`. The `resource` is `ibm_cdn`, and the `origin_type` property is set to `HOST_SERVER`.

   ```terraform
   resource "ibm_cdn" "test_cdn1" {
   hostname = "www.default.com"
   vendor_name = "akamai"
   origin_address = "111.111.111.5"
   origin_type = "HOST_SERVER"
   }
   ```
   {: codeblock}

1. After you finish building your configuration file, initialize the Terraform CLI. For more information, see [Initializing Working Directories](https://www.terraform.io/cli/init){: external}.

   ```terraform
   terraform init
   ```
   {: pre}

1. Provision the resources from the `main.tf` file. For more information, see [Provisioning Infrastructure with Terraform](https://www.terraform.io/cli/run){: external}.

   1. Run `terraform plan` to generate a Terraform execution plan to preview the proposed actions.

      ```terraform
      terraform plan
      ```
      {: pre}

   1. Run `terraform apply` to create the resources that are defined in the plan.

      ```terraform
      terraform apply
      ```
      {: pre}

1. From the [{{site.data.keyword.cloud_notm}} resource list](/resources){: external}, select the CDN instance that you created and note the instance ID.
1. Verify that the access policy is successfully assigned. For more information, see [Reviewing assigned access in the console](/docs/account?topic=account-assign-access-resources#review-your-access-console).

## What's next?
{: #terraform-setup-next}

Now that you successfully created your CDN instance with Terraform on {{site.data.keyword.cloud_notm}}, you can visit the [CDN Terraform registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/cdn){: external} to perform additional tasks using Terraform.
