---
id: schedule-search
title: Schedule a Search
sidebar_label: Get Started
description: When you save a search, you can add a schedule to run it at a regularly scheduled time, and add alerts.
---

import useBaseUrl from '@docusaurus/useBaseUrl';

When you save a search, you can add a schedule to run it at a regularly scheduled time, and add alerts. For alert types, see [Scheduled Searches](/docs/alerts/scheduled-searches). 

To run the scheduled search using receipt time save the search with receipt time enabled.

:::note
Scheduled searches with a **Real Time** run frequency do not support the **Use Receipt Time** option.
:::

## Schedule a search

You can create a scheduled search at the time you create a search, or edit a saved search later to add a schedule. 

1. Click **Schedule this search** on the edit dialog for a search.<br/>![schedule-this-search.png](/img/alerts/schedule-this-search.png)
1. **Run frequency.**<br/><img src={useBaseUrl('img/alerts/run-frequency-1.png')} alt="run frequency" width="670"/>
    * **Never.** Choose this option to temporarily **turn off a scheduled search**.
    * **Real Time.** Use this option to set up a [Real Time Alert](create-real-time-alert.md). Receipt time is not supported with a Real Time frequency.
    * **Every 15 Minutes**. May be delayed up to 5 minutes past the selected time or interval but will maintain the selected run frequency. The search will run every 15 minutes, but not exactly at :00, :15, :30, and :45.
    * **Hourly**. May be delayed up to 10 minutes past the selected time or interval but will maintain the selected run frequency
    * **Every 2, 4, 6, 8, or 12 Hours.** The search will run for the first time at the top of the hour you choose. * **Hourly.** The search will run every hour. We guarantee that hourly searches run every hour but not exactly at :00.
    * **Daily.** A Daily search will cover exactly 24 hours of activity. You can change the schedule whenever you'd like. Be aware that a scheduled search will run according to the time zone set on your computer at the time you configure the search. For example, if you are in San Francisco and set a search to run at 7:00 AM, it will run at 7:00 AM PST. If you then fly to New York, and your computer resets to EST, when you schedule a new search at 7:00 AM, it will run at 7:00 AM EST. These two searches will run at different times.
    * **Weekly**. The search will run every week. You may also select the day of the week that it runs and the time. Schedule may be delayed up to 10 minutes past the selected time or interval but will maintain the selected run frequency.
    * **Custom Cron**. Enter a custom CRON expression. The run frequency for a CRON expression must not be less than every 15 minutes. For details, see [Cron Examples and Reference](/docs/send-data/installed-collectors/sources/script-source/cron-examples-reference). 
    * For users in timezones that are +/- 30 minutes, the minute is based on UTC. So for customers in the IST timezone, there will be a 30-minute offset. So instead of starting at :00, it will be :30.
1. After you select a run frequency the popup refreshes. <br/><img src={useBaseUrl('img/alerts/save-item-after-run-frequency.png')} alt="Sumo Logic thumbnail logo" width="625"/>  
1. **Time range for scheduled search**. Indicates the time range your query will use to execute, which impacts the results generated by the query. Select the **Last 24 Hours**, to get a daily alert. Otherwise, select the time range you want the scheduled search to be run on. [Absolute time range](../../search/get-started-with-search/search-basics/time-range-expressions.md); for example, 06/10/2020 1:00:00 PM to 06/10/2020 2:00:00 PM is not allowed in Scheduled Searches and presents the message like this: `Invalid query. Static time range is not allowed for scheduled searches. `
    :::important
    This setting is different than the Time Range option configured for the Saved Search. The first time range is only used when you run the Saved Search from the Library. This Time Range applies to your Scheduled Search.
    :::
    Alternately type a time range; for example, -15m to run the search against data generated in the past 15 minutes. A time range outside the maximum allowed range for a given frequency is not allowed and presents the message like this:  

    `Invalid query. Max allowed time range for 15 minutes frequency is 1 day`  

    The maximum allowed time range for different scheduled search frequencies is as below:

    | Frequency          | Max Allowed Time Range |
    |:--------------------|:------------------------|
    | Real Time          | 15 minutes             |
    | 15 min             | 1 Day                  |
    | 15 min -1 hour     | 7 Days                 |
    | 1 hour - 3 hours   | 15 Days                |
    | 3 hour - 12 hours  | 30 Days                |
    | More than 12 hours | More than 30 days      |

    Consider adding an offset to your time range to ensure that all recent events are being indexed and scanned by the search. For example, a range of -20m -5m would represent a 15 minute time-range offset by 5 minutes.
1. **Timezone for scheduled search.** Select the time zone you would like your scheduled search to use. The schedule's time is based on this time zone. This time zone is not related to the time zone of your data. If you don't make a selection, the scheduled search will use the time zone from your browser, which is the default selection
1. **Send Notification.** Select the condition for when you want an alert sent.
   * **Every time a search is complete.** Select this option if you want an email with search results every time the search is run (depending on the frequency, you could get an email every 15 minutes, every hour, or once a day).
   * **If the following condition is met**. Select this option if you'd like to set up a scheduled search that alerts you to specific events.
     * **Number of results.** Depending on the search, set a condition to receive an email by the number of results. If your saved search returns log messages, then the alert will use the number of messages you specify. If your query produces aggregate results, the alert will use the number of rows or aggregates (or groups) and will not trigger on the number of raw results. For more control of your query, you can build in a threshold (for example `| where _count\> 30`) into the Search itself and set the alerts condition here to Greater than 0. That way the query will generate results if the expected condition is met. See this [FAQ](/docs/alerts/scheduled-searches/faq#real-time-alert-with-greater-than-1000-results) for an example.
       * **Equal to.** Choose if there is an exact number of records in a search result at which you want to be notified.
       * **Greater than.** Choose if you want to be notified only if the search results include greater than the number of messages or groups you set in the text box.
       * **Greater than or equal to.** Choose if you want to be notified only if the search results include greater than or equal to that number of messages or groups you set in the text box. For example, to ensure you're notified only when the specific query conditions are met, set the **Number of results** condition to greater than 0.
       * **Fewer than.** Choose if you want to be notified only if the search results include fewer than the number of messages or groups you set in the text box.
       * **Fewer than or equal to.** Choose if you want to be notified only if the search results include fewer than or equal to the number of messages or groups you set in the text box.
1. **Alert Type**. For details on the available alert types, see [Scheduled Search Alert Types](#scheduled-search-alert-types).

## Scheduled Search Alert Types

When you create a scheduled search, you can configure several different alert types including email, Script Action, ServiceNow Connection, Webhook, Save to Index, Real Time Alerts, and Cloud SIEM Signals.

### Email

You can create a scheduled search to alert you with an email when a set of conditions are satisfied. A maximum of 120 emails are sent per day per scheduled search. For instructions, see [Create an Email Alert](create-email-alert.md).

### Script Action

A Script Action is a Source type that receives data uploads triggered by a scheduled search. The script you create defines how data is consumed; for example, you could fire SNMP traps based on the result of the search. After setting up a Script Action, create a scheduled search. Each time the search query executes, the Collector runs the script configured in the Script Action. For instructions, see [Script Action](/docs/send-data/installed-collectors/sources/script-action).

:::note
You need the [View Collectors role capability](/docs/manage/users-roles/roles/role-capabilities) to alert with a Script Action.
:::

### ServiceNow Connection

Existing customers of both ServiceNow and Sumo Logic can now take advantage of the integration between the services. With this integration, search results from Sumo Logic are uploaded to your organization's ServiceNow account, allowing your organization to investigate issues across your deployment.

The main way data is uploaded to ServiceNow is through the use of scheduled searches. After saving a search, results are available in ServiceNow. Additionally, you can launch ad-hoc ServiceNow investigations using search results in Sumo Logic. For instructions, see [ServiceNow](/docs/alerts/webhook-connections/servicenow/).

### Webhook

Webhook connections allow you to send Sumo Logic alerts to third-party applications that accept incoming webhooks. For example, once you set up a Webhook connection in Sumo Logic, and create a scheduled search, then you can send an alert from that scheduled search as a post to a Slack channel, or integrate with third-party systems. For instructions, see [Scheduled Searches for Webhook Connections](/docs/alerts/webhook-connections/schedule-searches-webhook-connections).

### Save to Index

When you create a Scheduled Search, you can save the results to an Index. This way, your data can be searched at a later time using `_index=index_name` with increased search performance. For instructions, see [Save to Index](save-to-index.md).

### Save to Lookup

When you create a Scheduled Search, you can save the results to a [Lookup Table](../../search/lookup-tables/create-lookup-table.md). This way, you can view the results of the scheduled search from the Library by viewing the Lookup Table the search results were saved to. You can use the [lookup](/docs/search/search-query-language/search-operators/lookup) operator to enrich other log data with the information from the Lookup Table. For instructions, see [Save to Lookup](save-to-lookup.md).

### Real Time Alerts

Real Time Alerts are scheduled searches that run nearly continuously. That means that you're informed in real time when error conditions exist.

When an alert condition is satisfied, Sumo Logic sends an email (or triggers a script action). Sumo Logic examines ingested data in a rolling window using the Time Range you define. Any time a new result is found, another email is sent. For instructions, see [Create a Alert](create-real-time-alert.md).

### Cloud SIEM Signal

You can trigger the creation of a Cloud SIEM Signal with a scheduled search. Signals are otherwise generated when the conditions of a Cloud SIEM rule are satisfied by a Record. Signals are correlated with other Signals to create a [Cloud SIEM Insight](/docs/cse/get-started-with-cloud-siem/insight-generation-process/). For instructions, see [Generate Cloud SIEM Signals With a Scheduled Search](generate-cse-signals.md).

## FAQ

Important considerations:
* [How to Prevent your Scheduled Search from Timing Out](faq.md#how-to-prevent-your-scheduled-search-from-timing-out). Scheduled searches cannot run indefinitely. At some point, the query will be timed out to protect the reliability of the service.
* [Service Alert: Scheduled Search Email Quota Reached for Search](/docs/alerts/scheduled-searches/faq#service-alert-scheduled-search-email-quota-reached-for-search). Sumo Logic implements an email quota allowing 120 emails to be sent per day per scheduled search.
* [What Happens When a Scheduled Search Is Suspended?](faq.md#what-happens-when-a-scheduled-search-is-suspended) Learn what happens when a Scheduled Search is suspended.
* [Why Would a Scheduled Search Fail?](faq.md#why-would-a-scheduled-search-fail) Learn how to troubleshoot a failed Scheduled Search.

:::note
Fields are returned in lowercase in scheduled search results.
:::
