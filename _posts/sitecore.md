# Learnings from a DevOps Hackfest with Sitecore #
<p>Microsoft teamed up with Sitecore to identify and improve the company’s DevOps practices, with a focus on monitoring. Together they came up with a solution that provides multiple DevOps benefits, including Application Performance Monitoring, Automated Deployment, and Usage Telemetry.</p>
##Customer profile##
<p><a href="http://www.sitecore.net">Sitecore</a> is a global leader in experience management software. It offers a single .NET platform that combines web content management and digital marketing, optimizing technologies like personalization, automation, and analytics.</p>
<p>Customers can build and manage websites and mobile sites, run email campaigns, and publish content to social forums from a single interface.</p>
<table>
<thead>
<tr>
<th><p>Sitecore facts:</p>
<ul>
<li><p>Headquartered in Denmark</p></li>
<li><p>10,000+ Sitecore certified developers</p></li>
<li><p>800+ employees</p></li>
<li><p>4,500+ enterprise customers</p></li>
<li><p>$200 million+ annual revenue</p></li>
</ul></th>
<th><img src="images/sitecore.gif" align=center/></th>
</tr>
</thead>
<tbody>
</tbody>
</table>
<p>Sitecore has increased its investment in cloud services and is moving new and existing offerings to a Software as a Service (SaaS) model where the company is hosting the system for its customers. This introduces great potential as well as new challenges.</p>
##Problem statement##
<p>Although Sitecore has no official DevOps strategy, the company has been using several DevOps practices such as Continuous Integration and Delivery, Infrastructure as Code, and Automated Testing. Benefits include better transparency of processes and faster deliveries.</p>
<p>However, even with these DevOps practices in place, Sitecore believed there was room for improvement. With the move to cloud services, the load on operations was increasing as customers onboarded their SaaS offerings.</p>
<p>With this in mind, Sitecore joined forces with Microsoft in a Hackfest to identify potential improvement points in the SaaS pipeline.</p>
##Value Stream Map##
<p>To begin, the joint team of Microsoft and Sitecore used Value Stream Mapping (VSM), a means of identifying strengths and potential improvement points in an applications pipeline. This practice comes out of Lean Manufacturing and is easily adopted into IT service delivery to optimize processes and improve collaboration.</p>
<p>The focus of the VSM exercise is to map out what must take place in order to transform an idea into a product or service, noting all the necessary steps that need to be taken by product owners, development, test, operations, and others.</p>
<p><img src="images/sitecore-alm.png"/></p>This exercise should map out the average work-and-wait time between these different steps in an application development pipeline, showing a clear picture of how long each step takes to complete and how long before the next step can begin.</p>
<p>The team performed the Value Stream Mapping according to traditional Application Lifecycle Management (ALM) principles that cover Plan, Develop/Test, Release, and Monitor/Learn. By including DevOps practices, they could take it to the next level.</p>
<p>From this exercise, the Sitecore team learned that their current processes enabled them to deploy new features and fixes to production in less than 9 days if prioritized and that they had an average cadence of 26 days for deploying from regular sprints.</p>
<p>Below is the outcome of the VSM exercise for a specific Sitecore SaaS application. Using more traditional techniques like a whiteboard for the VSM work contributed to a great team experience and very visible, tangible results.</p>
<p><img src="images/sitecore-wb.png" align="center"/></p>
##Monitor and Learn##
<p>The Value Stream Mapping revealed an opportunity for Sitecore to improve how it monitors the solution and how it collects and uses feedback. It showed a lack of feedback from a service running in production to both product management and development. The Microsoft team recognized this as an issue because, in running online services, they’ve learned that understanding how people use those services is crucial to adoption and should be considered when deciding on new features.</p>
<p>Another important implementation of monitoring is to give the needed information at the right time. This is very relevant when troubleshooting an application, and it is critical to providing the right information to the team working on bug fixes.</p>
<p>Sitecore was monitoring the online service only for an up/down status and was collecting only very basic usage telemetry. With this conclusion, the team focused on two areas of monitoring:</p>
<ul>
<li><p><strong>Application Performance Monitoring.</strong> They wanted to get a better understanding of the running service by monitoring the performance of the application. They had learned from DevOps that understanding how apps are running is crucial to decreasing the time to recovery in case of a breakdown. It also helps ensure that users not only can access an application, but get the expected performance and experience from it as well.</p></li>
</ul>
<blockquote>
<p>With Application Performance Monitoring, a team can look deep into an application and collect detailed performance data and application exceptions, which engineers can use in optimizing and troubleshooting the service—providing the right information at the right time to the team working to fix an issue.</p>
</blockquote>
<ul>
<li><p><strong>Usage Telemetry.</strong> The team planned to collect Usage Telemetry to gather data about user behavior and application feature usage. DevOps has shown the importance of using the right data when making decisions about new and existing features in an application. This data is collected by monitoring usage of the running application and then analyzing it to understand how best to improve the app.</p></li>
</ul>
<p>Sitecore and Microsoft decided to hack a solution that would strengthen the feedback loop (Monitor and Learn) and provide both Usage Telemetry and deep application monitoring.</p>
##Sitecore Hackfest##
<p>The two companies chose to use <a href="https://azure.microsoft.com/en-us/services/application-insights/">Visual Studio Application Insights</a>, an Azure application telemetry service, to understand the usage, health, and availability of Sitecore’s services.</p>
<p>Application Insights offers automated web tests that can run from any Azure datacenter in the world to ensure availability for a variety of markets. A web test can easily be created from the portal. This works great for simple scenarios; however, in the case of Sitecore, the number of tests and properties of a test are very fluid and can change per tenant. Because of this, the hack team needed to be able to describe web tests programmatically. (For an in-depth look at how this was done, see the Sitecore technical case study <a href="http://dxdevblog.azurewebsites.net/developerblog/real-life-code/2015/12/04/Dynamic-Web-Test-Generation-Using-App-Insights.html">Dynamic Web Test Generation Using App Insights</a>.)</p>
##Overview of the solution##
<p>By using Application Insights, the team was able to describe web tests by an XML schema.</p>

`<WebTest Name="WebTest1" Id="0000-0000-0000-0000" Enabled="True" CssProjectStructure="" CssIteration="" Timeout="0" WorkItemIds=""`
` xmlns="http://microsoft.com/schemas/VisualStudio/TeamTest/2010" Description="" CredentialUserName="" CredentialPassword="" PreAuthenticate="True"`
` Proxy="default" StopOnError="False" RecordedResultFile="" ResultsLocale="">`
`<Items>`
`<Request Method="GET" Guid="a5f10126-e4cd-570d-961c-cea43999a200" Version="1.1" Url="http://microsoft.com" ThinkTime="0"`
` Timeout="300" ParseDependentRequests="True" FollowRedirects="True" RecordResult="True" Cache="False" ResponseTimeGoal="0"`
` Encoding="utf-8" ExpectedHttpStatusCode="200" ExpectedResponseUrl="" ReportingName="" IgnoreHttpStatusCode="False" />`
`</Items>`
`</WebTest>`
<p>For automating the deployment of web tests, Azure Resource Manager can deploy Application Insights test resources that are the type: Microsoft.Insights/webtests.</p>
<p>An Azure Resource Manager Application Insights might look like this:</p>
`{`
`"name": "Test1",`
`"apiVersion": "2015-05-01",`
`"type": "microsoft.insights/webtests",`
`"location": "Central US",`
`"tags": {`
`"[concat('hidden-link:', resourceId('microsoft.insights/components/appinsightsresource')))]": "Resource"`
`},`
`"dependsOn": [`
`"[concat('microsoft.insights/components/', parameters('appName'))]"`
`],`
`"properties": {`
`"Name": "Test1",`
`"Description": "A healthy descriptive piece of text",`
`"Enabled": true,`
`"Frequency": 300,`
`"Timeout": 30,`
`"Kind": "ping",`
`"Locations": [{ "Id": "us-il-ch1-azr" }],`
`"Configuration": {`
`"WebTest": "<WebTest Name="WebTest1" Id="0000-0000-0000-0000" Enabled="True" CssProjectStructure="" CssIteration="" Timeout="0" WorkItemIds=""`
` xmlns="http://microsoft.com/schemas/VisualStudio/TeamTest/2010" Description="" CredentialUserName="" CredentialPassword=""`
` PreAuthenticate="True" Proxy="default" StopOnError="False" RecordedResultFile="" ResultsLocale=""><Items><Request Method="GET"`
` Guid="a5f10126-e4cd-570d-961c-cea43999a200" Version="1.1" Url="http://microsoft.com" ThinkTime="0" Timeout="300" ParseDependentRequests="True"`
` FollowRedirects="True" RecordResult="True" Cache="False" ResponseTimeGoal="0" Encoding="utf-8" ExpectedHttpStatusCode="200"`
` ExpectedResponseUrl="" ReportingName="" IgnoreHttpStatusCode="False" /></Items></WebTest>"`
`},`
`"SyntheticMonitorId": "Test1"`
`}`
`}`
<p>For a full description of the technical implementation, see the Sitecore technical case study <a href="http://dxdevblog.azurewebsites.net/developerblog/real-life-code/2015/12/04/Dynamic-Web-Test-Generation-Using-App-Insights.html">Dynamic Web Test Generation Using App Insights</a>. A guide on Dynamic Web Test Creation is available at <a href="https://azure.microsoft.com/en-us/documentation/templates/201-dynamic-web-tests/">Azure Resources</a>. To get the Azure Resource Manager (ARM) template, go to the <a href="https://azure.microsoft.com/en-us/documentation/templates/">Azure Quickstart Templates</a> gallery.</p>
<p><strong>Conclusion and final words </strong></p>
<p>When the Hackfest ended after five days, the teams had implemented a solution that can be used across most of Sitecore´s online services and deployed as part of their existing methods.</p>
<p><img src="images/sitecore-appin.png" /></p>By adding a strong monitoring solution, Sitecore will be able to collect usage data from its online services to use in evaluation of upcoming improvements to its applications. This closes the incomplete feedback loop that was discovered by the Value Stream Map exercise.</p>
<p>Development and operations benefit from this solution on several levels:</p>
<ul>
<li><p><strong>Faster deployment.</strong> Sitecore can achieve this by incorporating monitoring needs in the planning phase and developing them together with other features. Then when operations receives new builds to deploy, testing already will be defined and deployment will be automated via ARM.</p></li>
<li><p><strong>Better communication.</strong> By having a strong monitoring solution, the development and operations teams can communicate better when bugs are found and fixes are needed. The Mean Time to Recovery or MTTR can be reduced when development and operations get a clear picture of what is failing or performing poorly.</p></li>
<li><p><strong>General application.</strong> By generalizing the newly implemented solution, it can be used across different services and be part of Sitecore´s application foundation. This will help all teams and reduce the time spent to develop and maintain the monitoring solution in the future.</p></li>
</ul>