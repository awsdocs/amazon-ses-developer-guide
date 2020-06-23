# General Amazon SES issues<a name="troubleshoot-general"></a>

The information on this page will explain and help diagnose issues that you may encounter when using Amazon SES\.

## Changes that I make are not immediately visible<a name="general-issues-1"></a>

As a service that is accessed through computers in data centers around the world, Amazon SES uses a distributed computing model called [eventual consistency](https://wikipedia.org/wiki/Eventual_consistency)\. Any change that you make in Amazon SES \(or other AWS services\) takes time to become visible from all possible endpoints\. Some of the delay results from the time it takes to send the data from server to server and from region to region around the world\. In the majority of cases, this delay will be no more than a few minutes\.

Some areas in which you may notice a delay include:
+ **Creating and modifying configuration sets** – When you create or modify a configuration set \(for example, if you [associate a dedicated IP pool with an existing configuration set](managing-ip-pools.md)\), there may be a brief delay from the time that you create or modify it to the time those changes are active\.
+ **Creating and modifying event destinations** – When you create or modify an event destination \(for example, [to tell Amazon SES to send your email sending data to another AWS service](monitor-using-event-publishing.md)\), there may be a delay between the time your created or modified the event destination and the time email sending events actually arrive at the specified destination\.