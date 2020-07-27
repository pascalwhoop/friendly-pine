---
title: Unique dashboards for external customers with Google Cloud
date: '2019-10-23T14:14:16.557Z'
excerpt: >-
  How BigQuery and DataStudio can enable you to build your managers requested
  dashboards right now
thumb_img_path: >-
  images/Unique-dashboards-for-external-customers-with-Google-Cloud/1*mLRhFJ0cXERllA7o2CRRLQ.jpeg
layout: post
---
How BigQuery and DataStudio can enable you to build your managers requested dashboards right now

[Publiq](https://www.publiq.be/), a client of ours wanted us to help them share their privacy-sensitive data insights with a number of cities in Belgium. The data describes how citizens participate in many events organized all over the country. It is stored in BigQuery and the cities each have an account with publiq. Each city should only see a dashboard based on “their data”, even though the entire dataset is driving Publiq’s publicly facing websites, such as [uitinvlaanderen.be](https://www.uitinvlaanderen.be/) *(*for more info on this client project, see the note at the bottom)*.*

This is a common scenario. While dashboards are widely used within organizations, they should also be possible to be shared with 3rd parties. Upstream supply chain partners may want to know the current workload of a factory to decide whether or not to ship more raw materials to the partner. Public organizations may want to share their performance metrics with the public and companies may want to share their KPIs with regulatory bodies.

Google’s GSuite offers a flexible way of sharing documents as every college student knows. What makes it cool though is the fact that this system also works for their data warehouse and reporting tools. Yes, with GCP it is as easy to share complex dashboards and metrics of a company as it is for a college student to share his lecture notes with her fellow students:

![](/images/Unique-dashboards-for-external-customers-with-Google-Cloud/1*mLRhFJ0cXERllA7o2CRRLQ.jpeg)

Lets look at how to get something like this set up with BigQuery and Data Studio in under an hour. For my example, I will use the Github public dataset, available on BigQuery. Below is a diagram that shows the target state

![](/images/Unique-dashboards-for-external-customers-with-Google-Cloud/1*fFwQa80tePUUengwKoDCog.png)

#### Big Query

Let’s say I want to create a dashboard for specific repositories for an number of clients. In my case, I want to show `torvalds/linux` to my work email and `apple/swift` to my personal email.

To define the mapping, I have a table which simply maps email addresses to repository names, as I want to expose certain repositories statistics to certain individuals.

Below is a SQL query that feeds a *view* which resides in a separate *dataset.* Big Query only offers IAM rules on a dataset level, hence the separate dataset. This dataset is now shared with all of the clients, using either their own google credentials or credentials which we create for them using the [GCP identity platform](https://cloud.google.com/identity-platform/). The `SESSION_USER()` function in SQL is where the magic happens: It returns the current session user’s email address.

![](/images/Unique-dashboards-for-external-customers-with-Google-Cloud/1*QPQ6TJLA2JoTlNe7000bXQ.jpeg)

Next, we need to share the “public dataset” (containing the saved view) with the users that we want to be able to read from the view and the view itself needs to be authorized to read from the “private dataset” i.e. our customer data table.

![](/images/Unique-dashboards-for-external-customers-with-Google-Cloud/1*zDHY_4C8bFbIvtjpGk01nQ.jpeg)

#### Google Data Studio

Hopping over to the data studio, we create a new data source and select the view. In the source settings, it is important to select “Viewer’s credentials” so that the viewers credentials are used when accessing the report’s underlying data.

![](/images/Unique-dashboards-for-external-customers-with-Google-Cloud/1*i481OlRYKgJGkppaKqNEkQ.jpeg)

![](/images/Unique-dashboards-for-external-customers-with-Google-Cloud/1*_cWA54Gtxcn_MhkL0_Ns1g.jpeg)

In the explorer, you can now build your dashboard as you please. I decided to build a small small dashboard that shows the different contributions of the top contributors of a repository (based on commit count). When I open the report in my two accounts (one part of our organization, one my personal account), I can see the two different repositories data and nothing else.

![](/images/Unique-dashboards-for-external-customers-with-Google-Cloud/1*DEsqt7SFnzodvA-FYfL4Xw.jpeg)

<figcaption>note the two user accounts in the top right of each&nbsp;window</figcaption>

* * *

#### Further reading

*   If you have a federated login provider configured (such as Auth0 or MS Active Directory), check the [SSO guide for GSuite](https://support.google.com/a/answer/60224?hl=en).
*   If you want to share dashboards with specific groups of people, simply create the groups in your organizations’ GSuite Admin settings
*   If you want to share dashboards with 3rd parties without giving them access to the underlying data, select “owner’s credentials” in the data source creation
*   To embed the reports on your website, check the [embedding help article](https://support.google.com/datastudio/answer/7450249?hl=en)
*   To avoid manually managing the “mapping table” `user_email_filter` , follow [this blogpost](https://medium.com/google-cloud/how-to-control-access-to-bigquery-at-row-level-with-groups-1cbccb111d9e) to see how they use a cloud function to synchronize the directory and a BigQuery table.
*   [Another public entity’s approach](https://radical-analytics.com/smart-data-studio-embedding-b36c6fc9a8ac) for sharing individual dashboards with their clients. Their approach doesn’t utilize Google’s identity but enables the sharing of the dashboard through links.

#### Context

Publiq vzw, our client mentioned above, collaborates with cities, municipalities and regions in Flanders, Belgium to collect all leisure activities and promote them in online and offline media. Their loyalty program “UiTPAS” provides privacy-sensitive insights in citizens’ leisure participation. All of this data is stored in BigQuery. They needed a solution to share the data for a specific city, only with employees of that city, in a single Google Data Studio report, using their own federated login provider (auth0) as Identity Provider.

* * *

*Starting in 2019,* [*Pascal*](https://pascalbrokmeier.de/about/) *is a data engineer and certified GCP cloud architect at Data Minded, who is passionate about delivering business value and actionable insights through well architected data products. Pascal studied Information Systems in Cologne and Sydney. He’s also been working part-time as a Software Engineer since 2012 for a consulting firm, a successful drone startup and a space company.*
