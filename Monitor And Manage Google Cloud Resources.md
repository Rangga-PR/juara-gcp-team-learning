## Cloud IAM

To use Monitoring, you must have the appropriate Identity and Access Management (IAM) permissions. In general, each REST method in an API has an associated permission. To use the method, or use a console feature that relies on the method, you must have the permission to use the corresponding method. Permissions aren't granted directly to users; permissions are instead granted indirectly through roles.

#### Project Level Roles

There are three primitives roles:

1. Editor: All viewer permissions, plus permissions for actions that modify state, such as changing existing resources.
2. Viewer: Permissions for read-only actions that do not affect state, such as viewing (but not modifying) existing resources or data.
3. Owner: All of editor permissions and permissions to Manage roles, permissions for a project and all resources within the project and Set up billing for a project.

#### How to Assign Project Level Role to user

1. Open Google Cloud Console
2. Navigate to Menu **IAM & Admin** > **IAM**
3. Click **+GRANT ACCESS** button
4. Set user and role

#### How to a Assign Cloud Storage Bucket Access

1. Create Bucket (Optional)
   a. Navigate to Menu **Cloud Storage** > **Buckets**
   b. Click **+CREATE** button
   c. Set bucket name and location
   d. Click **CREATE** button
   e. By default public access is prevented but you can set wether you want to allow it or not
2. Grant view access to user
   a. Navigate to Menu **IAM & Admin** > **IAM**
   b. Click **+GRANT ACCESS** button
   c. In a select a role field select **Cloud Storage** > **Storage Object Viewer** from dropdown
   d. Click **SAVE**

## Cloud Monitoring

Cloud Monitoring provides visibility into the performance, uptime, and overall health of cloud-powered applications. Cloud Monitoring collects metrics, events, and metadata from Google Cloud, Amazon Web Services, hosted uptime probes, application instrumentation, and a variety of common application components including Cassandra, Nginx, Apache Web Server, Elasticsearch, and many others. Cloud Monitoring ingests that data and generates insights via dashboards, charts, and alerts. Cloud Monitoring alerting helps you collaborate by integrating with Slack, PagerDuty, HipChat, Campfire, and more.

#### Create a Compute Engine Instance

1. Navigate to Menu **Compute Engine** > **VM Instances**
2. Click **Create Instance** button
3. Set instance' name, region and zone
4. Select instance' series and machine type
5. Select instance boot disk
6. Setup firewall config
7. Click **Create** button

#### Add Apache2 HTTP Server to your instance

1. Click **SSH** button in your selected instance to SSH into the instance
2. Install apache2 and php7 from shell
3. Start/restart apache2 service
4. Navigate back to console and select you instance click **External IP** to access apache2 default page

#### Install Monitoring and Logging agents

The Cloud Monitoring agent is a collected-based daemon that gathers system and application metrics from virtual machine instances and sends them to Monitoring. By default, the Monitoring agent collects disk, CPU, network, and process metrics.

1. SSH into your instance
2. Install agents by running this scripts:
   a. `curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh`
   b. `sudo bash add-google-cloud-ops-agent-repo.sh --also-install`

#### Create an Uptime Check

Create uptime checks to verify that a resource is always accessible.

1. Navigate to **Observability** > **Monitoring**
2. Click **Uptime Checks** and then click **Create Uptime Check** in the left panel
3. Set protocol to HTTP
4. Set resource type to instance and select your instance
5. Select check frequency and click **Continue**
6. In Response Validation, accept the defaults and then click **Continue**
7. In Alert & Notification, accept the defaults, and then click **Continue**
8. Set title and click **Test** to verify that your uptime check can connect to the resource
9. Click **Create**

#### Create an Alerting Policy

Use Cloud Monitoring to create one or more alerting policies.

1. Navigate to **Observability** > **Monitoring**
2. In the left panel, click **Alerting** and then click **+Create Policy**
3. Select a metric dropdown, Uncheck the Active and select on **VM instance** > **Interface** Select Network traffic and click **Apply**
4. Click **Next** and set the threshold position and threshold, value
5. Click **Next** and click **Manage Notification Channels**
6. Find channel type of email and click **ADD NEW** enter your desired email address and click **Save**
7. Select your new email notification channel
8. Set Alert name and Alert Message and click **Next**
9. Click **Create Policy**

#### Create a Monitoring Dashboard

display the metrics collected by Cloud Monitoring in your dashboards.

1. In the left menu select Dashboards, and then **+Create Dashboard**
2. Set dashboard name
3. Add chart
   a. Click **+ADD WIDGET**
   b. Under Visualization select your chart type
   c. Set widget name
   d. Select a metric. examples: CPU load
   e. Click **Apply**

#### View Instance Logs

1. Select **Navigation menu** > **Logging** > **Logs Explorer**
2. Select the logs you want to see and click **Resource**
3. Select **VM Instance** > **your instance name**
4. Click **Apply** and then click **Stream logs**

## Cloud Functions

A cloud function is a piece of code that runs in response to an event, such as an HTTP request, a message from a messaging service, or a file upload. Cloud events are things that happen in your cloud environment.

#### Create and Deploy a Function

1. Navigate to **Serverless** > **Cloud Run Functions**
2. Click **Create function**
3. Set environment and function name
4. Select function region and trigger type
5. Set Authentication config
6. Set Memory allocation and Autoscaling
7. In the Inline editor write your function and click **Deploy**
