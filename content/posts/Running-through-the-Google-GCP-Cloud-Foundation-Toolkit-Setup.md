---
title: Running through the Google GCP Cloud Foundation Toolkit Setup
date: '2020-04-26T21:29:48.684Z'
subtitle: An experience report of applying the CFT toolkit foundation steps
excerpt: An experience report of applying the CFT toolkit foundation steps
thumb_img_path: >-
  images/Running-through-the-Google-GCP-Cloud-Foundation-Toolkit-Setup/1*IHIS4TqLnnmZr9MaVa1mew.png
layout: post
---
![](/images/Running-through-the-Google-GCP-Cloud-Foundation-Toolkit-Setup/1*IHIS4TqLnnmZr9MaVa1mew.png)

Google released a [large set of terraform modules](http://github.com/terraform-google-modules/) to deploy a “best practice based” enterprise cloud setup using infrastructure as code, a common devops best practice. They call it the “[Cloud Foundation Toolkit](https://cloud.google.com/foundation-toolkit/)”. Along with this, they are also building an “[example foundation](https://github.com/terraform-google-modules/terraform-example-foundation)”. This article explains how we walked through applying this code to our organization and migrated existing projects into the new setup. The resulting code is a best-practice based cloud setup with the following out of the box

*   Audit exports to BigQuery
*   Billing exports to BigQuery
*   IAM groups for admin, billing and auditing
*   global VPC for prod and non-prod environment
*   ability to add new projects, link them to the global VPCs and billing in a few lines of code. Auditing, logging included
*   best practice based organizational policies
*   many more goodies as described in the various [modules](http://github.com/terraform-google-modules/)

* * *

#### Interlude: Motivation (skip if you know your stuff!)

Why are we doing this? Well, in short, we want to apply all the goodness of software engineering to infrastructure. If you think about it, software engineers are really good text file manipulators. Sounds pathetic I know. In essence though, that’s all most of us do. Writing emails? Text files. Social media? Textfiles and media attached to it. Code? Text files. Word documents? WYSIWYG text files. The list goes on. But SWE has one ultimate super-hero: Version control!

![](/images/Running-through-the-Google-GCP-Cloud-Foundation-Toolkit-Setup/0*eoYMgetihjIK2dYO.jpg)

When all the infrastructure is defined as a codified, machine-readable definition, automation reigns supreme! And if you have that definition be still somewhat human readable, well you get what we nerds call a programming language. Now, we can apply all the SWE tools! IDE’s, autocomplete, syntax highlighting and so on.

Look how beautifully the git flow maps to typical approval flows:

![](/images/Running-through-the-Google-GCP-Cloud-Foundation-Toolkit-Setup/1*8pc9dqU3mRLs7SZgEWRIlw.png)

Here’s a funny realization I had: You know comments right? These things that make sure you get what the hell the guy 2 years ago thought when he decided to put value X in field Y. These guys:

![](/images/Running-through-the-Google-GCP-Cloud-Foundation-Toolkit-Setup/1*-DfchZrofkaHdTWPj2iUuQ.jpeg)

Well, try putting a comment on a UI element in any of your favorite tools, I dare you.

![](/images/Running-through-the-Google-GCP-Cloud-Foundation-Toolkit-Setup/1*h73KLdqPeyHNulR1WHuk0A.jpeg)

I know right? Something as simple as a comment is infinitely harder in a GUI than in simple code files. Clicking your systems together may be easy for people who can’t read code but oh my, can you be productive if you can read code!

OK, enough motivation, let’s get into it

* * *

#### Pick your CI/CD

We like Github Actions. We also like *monorepos* or rather *product repos*. To us, the GCP cloud setup is a unit of deployable that should be considered one unit. Hence, we want this to be in one place. The example repo from Google assumes you’ll use several Cloud Code repositories, one per stage. It assumes 4 stages

    0 bootstrap  
    1 org  
    2 networks  
    3 products

Since we want to use Github and not rely on cloud build, I removed the cloud build module in `0-bootstrap/main.tf` . I also removed all cloud build yaml files. Instead we rely on github actions ([see the gist](https://gist.github.com/pascalwhoop/600d2c66687f37a9b09ac337cc30663d)). This has some impact on the way we went through the steps (see *1-org*) but if you use GCP cloud build, things should only get easier for you!

### Steps

![](/images/Running-through-the-Google-GCP-Cloud-Foundation-Toolkit-Setup/1*2zIcsfDY5Xb5cbBqc5LfEw.jpeg)

The above graphic skipped the bootstrap. I know, mean right? But the bootstrap is a one-time step while the other three steps will be adapted continuously (albeit less frequently at the bottom)

#### 0-Bootstrap

First, I forked the repo. However, we are using Github Actions and that means our service account key is added as a secret and exposed to terraform as an ENV variable. Since forks must be kept public, this couldn’t work. So, delete the fork and create a new repo instead. Otherwise, before you know it, someone will write a simple terraform snippet that extracts your sudo service account credentials and writes them to STDOUT, meaning it will end up in a public github comment. [Bad Idea](https://github.com/hashicorp/terraform-github-actions#secrets)! **So private repo it is.**

Next, I decided to unify some variables. Since we’re using a monorepo, symbolic links have proven a [good tool for us to keep terraform code DRY](https://medium.com/datamindedbe/avoiding-copy-paste-in-terraform-two-approaches-for-multi-environment-infra-as-code-setups-b26b7251cb11). A folder `universal` contains files that are, well, universal. For this project, that’s mainly `universal.auto.tfvars` and `providers.tf`. The variables below are all that’s needed to run through the `0-bootstrap` step

    org_id               = "Y"
    billing_account      = "X"
    group_org_admins     = "gcp-organization-admins@dataminded.be"
    group_billing_admins = "gcp-billing-admins@dataminded.be"
    audit_data_users     = "gcp-security-admins@dataminded.be"
    default_region       = "europe-west3"

So, as always

    terraform init  
    terraform plan  
    terraform apply

The first step however, is a bit special. After applying the code, a GCS bucket is created. Since the example repo comes with a commented out `backend.tf`, the README suggests to

1.  Copy the backend by running `cp backend.tf.example backend.tf` and update `backend.tf` with your bucket from the apply step (The value from `terraform output gcs_bucket_tfstate`)
2.  . Re-run `terraform init` agree to copy state to gcs when prompted

This is kinda neat. I have seen a lot of bootstrapping where people do a bunch of shell scripting to get started. This is cleaner.

This creates two projects (one for us, disabling the cloud build). More importantly, it creates a service account for which you can get an access key to use for the CI but also the manual applies for now.

    gcloud iam service-accounts keys create account.json --iam-account [org-terraform@gcp-foundation-seed-e3jd.iam.gserviceaccount.com](mailto:org-terraform@gcp-foundation-seed-e042.iam.gserviceaccount.com)

make sure you DO NOT commit this file to git. a quick

    echo "**/account.json" >> .gitignore

should suffice.

#### 1-Org

Alright, at this point I was motivated. It went smooth, next step!

    cd ../1-org  
    cat README.md

Here I hit the aforementioned “each step is a separate repo” discrepancy. But that’s fine, I like our approach. With the linked files from universal, the directory looks as such

    $ tree ./  
    ├── backend.tf  
    ├── folders.tf  
    ├── log_sinks.tf  
    ├── org_policy.tf  
    ├── projects.tf  
    ├── providers.tf -> ../_universal/providers.tf  
    ├── README.md  
    ├── terraform.tfvars  
    ├── universal.auto.tfvars -> ../_universal/universal.auto.tfvars  
    ├── variables.tf  
    └── versions.tf

Of course, you have to fix the `backend.tf` to point to the bucket created before. But the code is pretty explicity about that. Make sure to always read the README of each substep ;-)

Applying this code base however gave me trouble. We’re in Belgium, so we naturally went for `europe-west1` which is the Belgium data center. Well, that causes issues!

![](/images/Running-through-the-Google-GCP-Cloud-Foundation-Toolkit-Setup/1*4oHMGZSM2d-lXlxG2BdG-A.png)

<figcaption>Service availabilities for different EU&nbsp;regions</figcaption>

Turns out BigQuery is not supported in the Belgium region. Well, damn! Adjusting the `default_region` variable in the universal variables file means that the bootstrap step will be impacted! In fact, the state bucket will be impacted. But at this point, I have the `0-bootstrap` and the `1-org` state files in that bucket. **So choosing the wrong region in the beginning is annoying! Make sure you choose a region that supports all the services to save yourself the trouble.**

To fix this, I used the steps below

<script src="https://gist.github.com/pascalwhoop/ad20756c88d0d8757791f0d78e871a57.js"></script>

Now, we have the bucket in the new location and we can apply step `1-org`

    apply failed!

Damn, what now? Ah! Well, best to apply these changes with the aforementioned `account.json` account

    export GOOGLE_APPLICATION_CREDENTIALS=<path_to_account_json>

This should work right?

<iframe src="https://giphy.com/embed/Z5p44wwvR5ni/twitter/iframe" width="435" height="245" frameborder="0" scrolling="no"></iframe>

Turns out, they are using what GCP calls `service account impersonation` . This makes sense on cloud build but we’re already running these steps as the service account. It can’t impersonate itself right?

So I removed all occurences of `var.terraform_service_account` and voilà!

#### 2-Networks

Same steps as above, fix backend, link universal files, remove service account references. **init, apply. Done**

#### 3-Projects

Last step, wuhu! Defining which projects you want in your org. This is also the place where you can import any existing projects. The sample repo utilizes the [project-factory](https://github.com/terraform-google-modules/terraform-google-project-factory) module but it is still a [bit rough around the edges](https://github.com/terraform-google-modules/terraform-google-project-factory/issues/373). In brief, they are using local exec too much to create projects, relying on a local python, gcloud and jq installation.

Anyways, I’d think up a nice folder structure for your projects here and then apply the code. Aftewards, you can add existing projects to the repo by using the [terraform import functionality](https://www.terraform.io/docs/providers/google/r/google_project.html#import). New projects on the other hand can be created with the `project-factory` which will give you best-practices out of the box.

### Conclusion

The cloud foundation toolkit by Google allows any experienced engineer to set up an enterprise cloud setup in less than a day. But as always, that’s more of a sales pitch than a statement to rely on. **Do your homework and read the source code!** Also, don’t just blindly apply the best practices but also know what they are. Still, it’s nice not having to reinvent the wheel. The old pattern was that people share best practices in blog posts and technical guidelines and then you had to translate that into your specific environment and apply them. **Today, we’re taking a codified description of the best practices from the Google engineers themselves and then we apply that to our infrastructure!** I’m so happy to cut out the middle man (me) and focus on the really important things instead. Like our weekly facts and breakfast sessions or creating real business value for our clients

![](/images/Running-through-the-Google-GCP-Cloud-Foundation-Toolkit-Setup/1*XJMwuRGkxI2eTDTYCJtakw.jpeg)

<figcaption>(currently remote) facts and breakfast session at <a href="https://dataminded.be/" data-href="https://dataminded.be/" class="markup--anchor markup--figure-anchor" rel="noopener" target="_blank">Data&nbsp;Minded</a></figcaption>

* * *

Of course the obligatory, [we’re hiring](https://www.linkedin.com/posts/peeterskris_data-engineer-leuven-dataminded-activity-6658663850239504384-krEQ) and [contact us](https://www.dataminded.be/team)
