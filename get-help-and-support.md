---



copyright:
  years: 2017, 2025
lastupdated: "2025-06-27"

keywords:

subcollection: repo-name

---

{{site.data.keyword.attribute-definition-list}}

# Getting help and support for _serviceName_
{: #help-and-support}

If you experience an issue or have questions when using _serviceName_, you can use the following resources before you open a support case.
{: shortdesc}

* Ask a question in the [AI assistant](/docs/overview?topic=overview-ask-ai-assistant) from the console or the {{site.data.keyword.cloud_notm}} CLI.
* Review the [FAQs](/docs/linktoyourfaqtopic) in the product documentation.
* Review the [troubleshooting documentation](/docs/linktoyourfirsttroubleshootingtopic) to troubleshoot and resolve common issues.
* Check the status of the {{site.data.keyword.Bluemix_notm}} platform and resources by going to the [Status page](/status){: external}.
* Review [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-cloud){: external} to see whether other users experienced the same problem. When you ask a question, tag the question with `ibm-cloud` and `service-Name`, so that it's seen by the {{site.data.keyword.Bluemix_notm}} development teams.

If you still can't resolve the problem, you can open a support case. For more information, see [Creating support cases](/docs/account?topic=account-open-case). And if you're looking to provide feedback, see [Submitting feedback](/docs/overview?topic=overview-feedback).

## Providing support case details
{: #support-case-details}

To ensure that the support team can start investigating your case to provide a timely resolution, you must include detailed information along with steps to re-create the issue, if applicable. Review the following types of information to include in your support case for issues with _serviceName_.

1. Provide your transit gateway ID:

   * Run `ibmcloud tg gateways` to get the ID for the transit gateway in question from the output, and then collect the output of these commands `ibmcloud tg gateway GATEWAY_ID` and `ibmcloud tg connections GATEWAY_ID`.
   * In the output of the previous command, get the connection IDs for the connections to the relevant VPCs, and if relevant, the classic infrastructure connection. Also, collect the output of the following command against each connection ID: `ibmcloud tg connection GATEWAY_ID CONNECTION_ID`.
   * Ping and trace results for connectivity issues.

2. Provide a network diagram with your specific setup.
3. List your source and destination IP addresses.
