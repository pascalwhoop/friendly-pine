---
title: >-
  Cloud story: Spin down unneeded infrastructure quickly with terraform to save
  OPEX
date: '2020-03-24T10:13:16.988Z'
subtitle: "The benefit of infrastructure as code and cloud: If you don’t need it, don’t pay for\_it."
excerpt: >-
  The benefit of infrastructure as code and cloud: If you don’t need it, spin it
  down.
thumb_img_path: >-
  images/Cloud-story--Spin-down-unneeded-infrastructure-quickly-with-terraform-to-save-OPEX/1*jMV5f5Nd5sS_LO3_Nx5PIA.jpeg
layout: post
---
![](/images/Cloud-story--Spin-down-unneeded-infrastructure-quickly-with-terraform-to-save-OPEX/1*jMV5f5Nd5sS_LO3_Nx5PIA.jpeg)

<figcaption>Photo by <a href="https://unsplash.com/@osmanrana" data-href="https://unsplash.com/@osmanrana" class="markup--anchor markup--figure-anchor" rel="noopener" target="_blank">Osman Rana on&nbsp;Unsplash</a></figcaption>

The COVID-19 pandemic puts many companies in trouble. Some need to **save costs where possible**. Development projects may completely stop until cash flow improves. In one case, we were asked to see what savings we can offer to a client’s infrastructure. They use Kubernetes and Kafka as well as our custom datalake frameworks based on Apache Spark and AWS glue.

Since we have **all infrastructure defined as code**, we are able to to spin down components that aren’t needed. We actually disabled the TST and UAT environment completely and reduced the DEV environment to just the bare minimum, spinning up the K8S and Kafka cluster only when needed.

The **production environment is kept running to support existing use cases**. But since no new deployments (except for hot-fixes) will be done, we can also scale down here. Adjusting all applications CPU and memory requests and limits to measured values according to [vertical pod autoscaling](https://cloud.google.com/kubernetes-engine/docs/how-to/vertical-pod-autoscaling).

This can lead to **savings of more than 75% of infrastructure costs**. Shutting down 3 out of 4 environments and reducing the costs of the production environment can be achieved very quickly thanks to infrastructure as code.

It is these moments that show the value of cloud and the investments made into defining the infrastructure in code. This would simply not be possible with a traditional on premise setup.

Let’s compare this to an on-premise installation, where hardware was purchased years in advance. Or a more naive cloud approach which uses a set of VMs that need to be running 24/7. These architectures and their costs would remain more or less static, regardless of the load. The nice thing about cloud-native architectures is that they not only easily scale up, but can also easily scale down.

#### Gotchas we faced

*   **AWS MSK doesn’t scale down. Only up**. We decided to delete the entire cluster because it’s not needed and doesn’t contain valuable data. All data stays in production
*   Deployments on top of our K8S may have created RDS managed network interfaces. Since our core infrastructure code is separate from the application infrastructure code, we ran into security group deletion issues. Manually removing these in the console resolves the issues. **Documenting the process is key here**
*   What if someone else has to spin this back up? **Creating a screen recording of the process helps giving an exact picture** of what happened to whoever comes after you.

* * *

Lets hope for a quick recovery of Europe and all the other countries affected by this pandemic. When it’s time, we’ll run `terraform apply` again and bring things back up quickly. Then the feature teams can pick up where they left off and redeploy their applications and services to the platform.

Facing similar issues? [You know how to find us](https://www.linkedin.com/company/data-minded/people/)
