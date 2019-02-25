---

copyright:
  years: 2018
lastupdated: "2018-08-09"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Auditing virtual server events with Activity Tracker

You can audit a virtual server instance through its life cycle by using [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov). You must have an instance of Activity Tracker with the premium service plan provisioned in US South. The premimum plan gives you access to the Kibana dashboard that has more options for filtering and searching audit logs.

The logs are visible in the account logs section for the owner of the {{site.data.keyword.cloud}} account. The account owner can view the following logs:
* Events triggered by the account owner.
* Events triggered by users that are linked with the account.

You can audit the following virtual server instance events through Activity Tracker:
* Provisioning a virtual server instance
* Disabling a port for a private or public interface
* Enabling a port for a private or public interface
* Setting port speed
* Creating an image template
* Powering off a virtual server instance
* Powering on a virtual server instance
* Rebooting a virtual server instance
* Renaming a virtual server instance
* Entering rescue mode for a virtual server instance
* Adding a security group to a public or private interface of a virtual server instance
* Removing a security group from a public or private interface of a virtual server instance
* Loading a virtual server instance from an image
* Booting a virtual server instance from an image
* Canceling a virtual server instance device

For events associated with a network interface, the audit log does not distinguish if the event occurred on a public interface or a private interface.
{: tip}
