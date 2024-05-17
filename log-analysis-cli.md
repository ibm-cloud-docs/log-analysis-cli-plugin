---
 
copyright:
  years:  2021, 2024
lastupdated: "2024-05-17"

subcollection: log-analysis-plugin-cli

keywords: IBM Cloud Logging CLI, IBM Cloud Logging command line, IBM Cloud Logging terminal, IBM Cloud Logging shell

---

{{site.data.keyword.attribute-definition-list}}


# Logging (ibmcloud logging) CLI
{: #log-analysis-cli}

The {{site.data.keyword.cloud}} command-line interface (CLI) provides extra capabilities for service offerings. This information describes how you can use the CLI to list and export information for {{site.data.keyword.la_full_notm}} service instances for an account.
{: shortdesc} 

As of 28 March 2024 the {{site.data.keyword.la_full_notm}} and {{site.data.keyword.at_full_notm}} services are deprecated and will no longer be supported as of 30 March 2025. Customers will need to migrate to {{site.data.keyword.logs_full_notm}}, which replaces these two services,  prior to 30 March 2025. {{site.data.keyword.logs_full_notm}} will become generally available during the summer of 2024 in Frankfurt and Madrid with day-one support for EU-managed controls. The service will continue its worldwide multizone region (MZR) roll-out through 3Q2024.
{: deprecated}

Exporting using the UI uses the [V1 export API](https://cloud.ibm.com/apidocs/log-analysis#export){: external} and is limited to the functionality provided by that version of the API.
{: important}

## Prerequisites
{: #log-analysis-cli-prereq}

* Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started).
* Install the Logging CLI by running the following command:

   ```sh
   ibmcloud plugin install logging
   ```
   {: pre}

You're notified on the command line when updates to the {{site.data.keyword.cloud_notm}} CLI and plug-ins are available. Be sure to keep your CLI up to date so that you can use the latest commands. You can view the current version of all installed plug-ins by running `ibmcloud plugin list`.
{: tip}



## ibmcloud logging service-instances
{: #log-analysis-service-instances}

Use this command to list the service instances for {{site.data.keyword.la_full_notm}}. 

```sh
ibmcloud logging service-instances [OPTIONS]
```
{: pre}


### Command options 
{: #log-analysis-service-instances-options}

`--service-name <NAME>` | `--sn <NAME>`
:   Name of the service.

`--region <NAME>` | `-r <NAME>`
:   Name of the region, for example, `us-south` or `eu-gb`. If not specified, the region logged into or targeted will be used.

`--all-regions`
:   Services hosted across all regions.

`-g <GROUP>`
:   Resource Group associated with the hosted service.

`--all-resource-groups`
:   Services hosted across all resource groups.

`--quiet` | `-q`
:   Supresses verbose output.

`--output <TYPE>`
:   A comma-separated list of output preferences enclosed in double-quotes (").  If only a single preference is specified, the double-quotes can be omitted. Supported options are `WIDE` and `JSON`.  

    If `JSON` is specified, output will be returned in JSON format.  If `JSON` is not specified, output will be returned in a tabular format.

    `WIDE` returns additional details in the output.

`--help` | `-h`
:   List options available for the command.

  
### Examples
{: #log-analysis-service-instances-examples}

The following are examples using the **`ibmcloud logging service-instances`** command.

List all logging service instances.

```sh
ibmcloud logging service-instances
```
{: pre}

List all instances that are in the `test-rg` resource group.

```sh
ibmcloud logging service-instances -g test-rg
```
{: pre}

List all instances and include additional details, such as ID, GUID, and Resource ID.

```sh
ibmcloud logging service-instances --output wide
```
{: pre}

List all instances and include only the minimal details of Name, Region and State.

```sh
ibmcloud logging service-instances --quiet
```
{: pre}

List all instances for the `us-south` region.

```sh
ibmcloud logging service-instances --region us-south
```
{: pre}

List all instances in the `us-south` region and returns the output in JSON format.

```sh
ibmcloud logging service-instances --region us-south --output json
```
{: pre}

<!-- ===================================== -->

## ibmcloud logging export
{: #log-analysis-export}

Use this command to export log information. Options are provided to filter the exported log information. Exported information is presented as a response to the command and can optionally be accessed by a link sent to an email address provided on the command.

```sh
ibmcloud logging export --service-key <SERVICE_KEY> [OPTIONS]
```
{: pre}


### Command options 
{: #log-analysis-export-options}


`--service-key <SERVICE_KEY>` | `-s <SERVICE_KEY>`
:   (REQUIRED) Service key for the instance.

`--region <REGION>` | `-r <REGION>`
:   Name of the region, for example, `us-south` or `eu-gb`. If not specified, the region logged into or targeted will be used.

`--hosts <HOST_NAMES>` | `--ho <HOST_NAMES>`
:   A comma-separated list of host names enclosed in double-quotes (").  If only a single host name is specified, the double-quotes can be omitted.  Log entries for the specified host names will be returned. 

`--apps <APP_NAMES>` | `-a <APP_NAMES>`
:   A comma-separated list of app names enclosed in double-quotes (").  If only a single app name is specified, the double-quotes can be omitted.  Log entries for the specified apps will be returned.

`--levels <LOG_LEVELS>` | `-l <LOG_LEVELS>`
:   A comma-separated list of log level values enclosed in double-quotes (").  If only a single level is specified, the double-quotes can be omitted.  Log entries with the specified level will be returned.  Depending on your environment log levels can include: CRITICAL, DEBUG, EMERGENCY, ERROR, FATAL, INFO, SEVERE, TRACE, WARN, or ALERT.

`--number <NUMBER>` | `-n <NUMBER>`
:   The total number of log entries to be exported.

`--from <TIME>`
:   The starting time to be used for log entries.  No log entries early than this time will be returned.  `<TIME>` is specified as a UNIX timestamp in seconds or milliseconds. 

`--to <TIME>`
:   The ending time to be used for log entries.  No log entries after than this time will be returned.  `<TIME>` is specified as a UNIX timestamp in seconds or milliseconds.

`--output <TYPE>`
:   Type of output produced.  If `JSON` is specified, output will be returned in JSON format.  If not specified, output will be returned in tabular format.

`--query <STRING>` | `-q <STRING>`
:   A value to be searched for within the log.  All log entries containing the specified `<STRING>` will be returned.

`--prefer <VALUE>` | `-p <VALUE>`
:   The log lines you want to export.  Valid values are `HEAD` and `TAIL`.  `HEAD` specifies the log lines will be exported from the earliest entry to the most current entry.  `TAIL` specifies the log lines will be exported in reverse order from the most current entry to the earliest entry. If not specified, the default is `TAIL`.

`--email <ADDRESS>` | `-e <ADDRESS>`
:   If specified, an email with a link to the export information will be sent to the specified email `<ADDRESS>`.  The recipient can use that link to download the exported information.

`--email-subject <SUBJECT>` | `--es <SUBJECT>`
:   Use with `--email` to specify a subject line to be included in the email that is sent.

`--help` | `-h`
:   List options available for the command.
  
### Examples
{: #log-analysis-export-examples}

The following are examples using the **`ibmcloud logging export`** command.

Export all logs for the provided service key.

```sh
ibmcloud logging export --service-key <SERVICE_KEY>
```
{: pre}

Export log entries for the provided service key occurring during the specified time range.  Time stamps need to be in UNIX format.  For example: `1614228407550`.

```sh
ibmcloud logging export --service-key <SERVICE_KEY> --to <END_TIME> --from <START_TIME>
```
{: pre}

Export `INFO` and `ERROR` level log entries for the provided service key. 

```sh
ibmcloud logging export --service-key <SERVICE_KEY> --levels "INFO,ERROR"
```
{: pre}

Export log entries for the `metric-server` and `myapp` apps for the provided service key.
 
```sh
ibmcloud logging export --service-key <SERVICE_KEY> --apps "metrics-server,myapp"
```
{: pre}

Export log entries for the `test-hostname` and `cloudantnosqldb` hosts for the provided service key.  

```sh
ibmcloud logging export --service-key <SERVICE_KEY> --hosts "test-hostname,cloudantnosqldb"
```
{: pre}

Export log entries for the `eu-gb` region for the provided service key. 

```sh
ibmcloud logging export --service-key <SERVICE_KEY> --region eu-gb
```
{: pre}

Export log entries containing the string "new line" for the provided service key. 

```sh
ibmcloud logging export --service-key <SERVICE_KEY> --query "new line"
```
{: pre}

Export log entries containing the string "new line" in the first log lines (`head`) for the provided service key.

```sh
ibmcloud logging export --service-key <SERVICE_KEY> --query "new line" --prefer head
```
{: pre}

Export log entries in JSON format for the `myapp` app for the provided service key.  

```sh
ibmcloud logging export --service-key <SERVICE_KEY> --apps myapp --output json
```
{: pre}

Send an email with a downloadable link to `myemail@mycompany.com` with the subject "Emailing myapp logs".  In this example the link will let `myemail@mycompany.com` see the list of all log entries for the `myapp` app for the provided service key at the time the command was run.  

```sh
ibmcloud logging export --service-key <SERVICE_KEY> --apps myapp --email myemail@mycompany.com --email-subject "Emailing myapp logs"
```
{: pre}




