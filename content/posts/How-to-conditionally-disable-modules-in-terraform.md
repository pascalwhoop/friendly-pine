---
title: How to conditionally disable modules in terraform
date: '2019-09-27T08:04:41.133Z'
excerpt: >-
  Terraform doesn’t support the count parameter on modules. A proposal was made
  for a enabled parameter, but this is also not yet present…
thumb_img_path: >-
  images/How-to-conditionally-disable-modules-in-terraform/1*qLtZOOzexofoGUMGIhErhA.jpeg
layout: post
---
![](/images/How-to-conditionally-disable-modules-in-terraform/1*qLtZOOzexofoGUMGIhErhA.jpeg)

<figcaption>Photo by <a href="https://unsplash.com/@m_k_nd" data-href="https://unsplash.com/@m_k_nd" class="markup--anchor markup--figure-anchor" rel="noopener" target="_blank">Mukund Nair</a> on&nbsp;Unsplash</figcaption>

**Update: Terraform 0.13 allows you do add a** `**count**` **to a** `**module**`** . So if you can upgrade, do so and skip this article!  
**[**https://github.com/hashicorp/terraform/blob/guide-v0.13-beta/README.md**](https://github.com/hashicorp/terraform/blob/guide-v0.13-beta/README.md)

* * *

Terraform doesn’t support the `count` parameter on modules. A proposal was made for a `enabled` parameter, but this is also not yet present. Therefore, it appears to be hard to conditionally disable terraform modules. There’s a dirty hack to put `count` parameters on all the resources inside the module. Yet, this is veeery messy.

If we take a step back and think about when we actually want to do this, it is typically a use case for e.g. a dev environment. These environments have a different `tfvars` input file associated with them and therefore a developer may want to do something like

    enabled = var.env == "prd" ? true : false

Yet, this is not possible in terraform as of today. Yet, below is a solution for still enabling / disabling modules for different environments.

See my [other post](https://medium.com/datamindedbe/avoiding-copy-paste-in-terraform-two-approaches-for-multi-environment-infra-as-code-setups-b26b7251cb11) for an overview of our folder structure for several environments which enables this trick. Our current setup is also shown below:

    .  
    ├── deploy.sh  
    ├── destroy.sh  
    ├── dev  
    │   ├── infra_override.tf  
    │   ├── infra.tf -> ../universal/infra.tf  
    │   ├── output.tf -> ../universal/output.tf  
    │   ├── state.tf  
    │   ├── terraform.tfvars  
    │   └── variables.tf -> ../universal/variables.tf  
    ├── prd  
    │   ├── infra.tf -> ../universal/infra.tf  
    │   ├── output.tf -> ../universal/output.tf  
    │   ├── state.tf  
    │   ├── terraform.tfvars  
    │   └── variables.tf -> ../universal/variables.tf  
    ├── symlink.sh  
    ├── tst  
    │   ├── infra_override.tf  
    │   ├── infra.tf -> ../universal/infra.tf  
    │   ├── output.tf -> ../universal/output.tf  
    │   ├── state.tf  
    │   ├── terraform.tfvars  
    │   └── variables.tf -> ../universal/variables.tf  
    └── universal  
        ├── infra.tf  
        ├── output.tf  
        └── variables.tf

The `infra.tf` file contains all the code we share across environments. Let’s say we have a module “gitlab” which we don’t want to deploy to an environment to preserve resources.

First, we define the module as usual in our `infra.tf` file.

    module "gitlab" {  
     **source = "../../modules/gitlab"**  
    	artifacts_bucket = module.account.artifacts_bucket  
    	cicd_role = local.cicd_role  
    	eks_cluster_name = module.kubernetes.k8s_cluster_name  
    	environment = var.environment  
    	...  
    }

Now, in the `infra_override.tf` file of our `dev/tst/...` environments, we define the following lines to avoid deploying the module to these environments

    #no deployment of gitlab on the tst cluster needed  
    module "gitlab"{  
     **source = "../../modules/empty-module"**  
    }

This [override file](https://www.terraform.io/docs/configuration/override.html) gets merged with the `infra.tf` file by terraform. The `empty-module` is exactly what it suggests. A folder with an empty `nothing.tf` file, simply explaining why it exists.

**Warning Edit:** I realized this after posting this but the empty module actually requires a file that defines the same `variable` definitions as the original module. This means one must create an “empty” module version for each module that one wants to conditionally enable. I suggest putting an `empty` folder inside of the module code that links to the parent’s `vars.tf` file.

**Result:** The ability to “conditionally enable a terraform module”. It’s a hack, but it works and it’s pretty easy to understand.
