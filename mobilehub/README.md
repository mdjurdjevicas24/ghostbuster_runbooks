# MobileHub Runbook

## Setup Info:
* LIVE Mobilehub app server machines (Apache & PHP):  `lappmav001 – 016`.
* 1% of the traffic is routed to `lapphav001` where we have code tracing available. Can be accessed via Live terminal (`lteropv001`): http://xhgui-mobileapp.as24.local/
* 3 different databases (running in master slave replication, one master each database, multiple slaves for each master) (`lmyqmav001 - 005 & lmyqmtv001 & 002`)
* 2 redundant servers for memcache (`lmemmav001 & 002`)

## Diagnostic tools

* [Icinga](https://icinga2.admin.autoscout24.com/dashboard)
    * Search for „mobilehub“ and check if all checks are green
* [MobileHub Selftest](http://apps.scout24.com/HealthCheck/?info)
    * Search for „mobilehub“ and check if all checks are green
    * Shows all external dependencies (if green they everything available)
    * Errors are shown in red, URL should point you to the affected system

* [MobileHub Dashboard -> Serverstatus](http://hubadmin.as24.local/?panel=serverstatus)

    * Login (LDAP credentials, GS24 account)
    * Check the errors with the highest number (Known issue:  “Service: FindArticles SoapFault exception: [s:Internal error] Make / model parameters list is too big in” due to a bug in the iOS app)
    * If the errors are not pointing to specific issues, consider deleting all errors to see only the newest (Clear button at the bottom)

* [Rest Assured Tests](https://teamcity.as24.local/viewType.html?buildTypeId=MobileApps_As24MobileHubRestTests_LiveRestTests)
    * Check if all tests are green. Failing tests might indicate where we have a problem.

* [Splunk](https://live-splunk.gs24.com/de-DE/app/search/search?s=%2FservicesNS%2Fnobody%2Fsearch%2Fsaved%2Fsearches%2FMobileHub%2520Hub%2520500er%2520last%252024h&sid=1505734988.9231268&q=search%20index%3D%22mobilehub%22%20host%3D%22lappmav*%22%20source%20!%3D%20%22*healthcheck.log%22%20.ERROR%20NOT%20(%22Too%20many%20lastSyncedVehicleIds%20articles%22%20OR%20%22Too%20many%20currentVehicleIds%20articles%22%20OR%20%22Authentication%20failed%20for%20username%22)&earliest=-24h%40h&latest=now)
    * [Query for Hub Errors](https://live-splunk.gs24.com/de-DE/app/search/search?s=%2FservicesNS%2Fnobody%2Fsearch%2Fsaved%2Fsearches%2FMobileHub%2520Hub%2520500er%2520last%252024h&sid=1505734988.9231268&q=search%20index%3D%22mobilehub%22%20host%3D%22lappmav*%22%20source%20!%3D%20%22*healthcheck.log%22%20.ERROR%20NOT%20(%22Too%20many%20lastSyncedVehicleIds%20articles%22%20OR%20%22Too%20many%20currentVehicleIds%20articles%22%20OR%20%22Authentication%20failed%20for%20username%22)&earliest=-24h%40h&latest=now)
    * Most important filters in splunk: `index="mobilehub" host="lappmav*`

* [Catchpoint](https://portal.catchpoint.com/ui/Entry/SingleSignOn.aspx)
    * Sign in with scout24 email (`firstname.lastname@scout24.com`), Namespace `scout24.com`
    * Go to Tests -> MobileApp -> Mobilehub vehiclelist

* [Ganglia](https://ganglia.admin.autoscout24.com/)
    * currently not working :(





