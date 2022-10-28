# Assigning IP pools in Amazon SES<a name="managing-ip-pools"></a>

You can use IP pools to create groups of dedicated IP addresses for sending specific types of email\. You can also use a pool of IP addresses that are shared by all Amazon SES customers\.

When assigning an IP pool to a configuration set, you can choose from the following options:
+ *A specific dedicated IP pool* – When you select an existing dedicated IP pool, emails that use the configuration set are sent using only the dedicated IP addresses that belong to that pool\. For procedures on how to create: 
  + new *standard* IP pools, see [Creating standard dedicated IP pools for dedicated IP addresses \(standard\)](dedicated-ip-pools.md)\.
  + new *managed* IP pools, see [Creating a managed IP pool to enable dedicated IPs \(managed\)](managed-dedicated-sending.md#dedicated-ip-pools-mds)\.
+ **ses\-default\-dedicated\-pool** – This pool contains all of the dedicated IP addresses for your account that do not already belong to an IP pool\. If you send an email using a configuration set that is not associated with a pool, or if you send an email without specifying a configuration set at all, the email is sent from one of the addresses in the default pool\.
+ **ses\-shared\-pool** – This pool contains a large set of IP addresses that are shared among all Amazon SES customers\. This option may be useful when you need to send email that doesn't align with your usual sending behaviors\.

## Assigning an IP pool to a configuration set<a name="assign-ip-pools"></a>

This section references the procedures for assigning and modifying IP pools in a configuration set using the Amazon SES console\. 
+ **To assign an IP pool to a configuration set using the console**\.\.\.
  + **while creating a new configuration set** – see [Sending IP pool](creating-configuration-sets.md#create-config-set-step-4) in Step 4 of [Create configuration sets](creating-configuration-sets.md)
  + **while modifying an existing configuration set** – select the **Edit** button in the **General details** panel of the selected configuration set, and follow the directions for [Sending IP pool](creating-configuration-sets.md#create-config-set-step-4) in Step 4 of [Create configuration sets](creating-configuration-sets.md)