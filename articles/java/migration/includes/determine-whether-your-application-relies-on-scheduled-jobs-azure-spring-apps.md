---
author: KarlErickson
ms.author: karler
ms.date: 7/21/2020
---

#### Determine whether your application relies on scheduled jobs

Scheduled jobs, such as Quartz Scheduler tasks or Unix cron jobs, should NOT be used with Azure Spring Apps. Azure Spring Apps will not prevent you from deploying an application containing scheduled tasks internally. However, if your application is scaled out, the same scheduled job may run more than once per scheduled period. This situation can lead to unintended consequences.

Inventory any scheduled tasks running on the production server(s), inside or outside your application code.
