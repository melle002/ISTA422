# Azure Fun. Ch 1, Cloud Basics
## Homework


##### 1. What are the differences between Iaas, Paas, and Saas?
* SaaS is software that is centrally hosted and managed for the end customer.
* With PaaS, you deploy your application into an application-hosting environment provided by the cloud service vendor.
* An IaaS cloud vendor runs and manages server farms running virtualization software, enabling you to create VMs that run on the vendor’s infrastructure.


##### 2. What is the Azure Service Management (ASM) deployment model? What is the Resource Manager deployment model?
* Since it went into public preview, the Azure Service Management (ASM) deployment model has been used to deploy services. In the Azure portal, services managed with ASM are referred to as classic
* In 2015, Microsoft introduced the Resource Manager deployment model as a modern, more functional replacement for ASM.

##### 3. What is the difference between a control plane amd a data plane?
control planes are used to control services, not just to deploy them. This is different from a data plane, which manages the data used by a service.
##### 4. What is Role-Based Access Control?
provides fine-grained control over the operations and scope with which a user can perform a control-plant action.
##### 5. Why would you want to create a custom role for role-based access control?
If none of the built-in roles and no combination of the built-in roles provides exactly what you need, you can create a custom role.
##### 6. Consider the Azure portal. What is the dashboard? What is the hub? What is a blade?
* Dashboard- The area on the right with the tiles is called your Dashboard. You can customize this by adding tiles, removing tiles, resizing tiles, and so on by selecting Edit Dashboard.
* Hub- The column on the left is called a hub; it shows you a core set of options such as Resource Groups, All Resources, and Recent. The other items on this hub are resources you have selected and/or used before.
* Blade- The separate sections that get opened are called blades.

##### 7.Access the conceptual Azure documentation on Github. Search the documentation and answer this question: What happens when I reach the spending limit?
If you exceed the credit amount, your service will be disabled until the next month starts. You can turn off the spending limit and add a credit card to be used for the additional costs.
##### 8. Access the Azure samples on Github. Search for an example that will allow you to download an Azure invoice. Copy the source code to your discussion. (This is Program.cs.)
using System;

using System.Diagnostics;

using System.Threading.Tasks;

using System.Collections.Generic;

using System.Linq;

using System.IO;

using System.Runtime.InteropServices;

using Microsoft.Extensions.Configuration;



// Azure Management dependencies

using Microsoft.Rest.Azure.Authentication;

using Microsoft.Azure.Management.Billing;

using Microsoft.Azure.Management.Billing.Models;



namespace billing_dotnet_invoice_api

{

    class Program

    {

        public static IConfigurationRoot Configuration { get; set; }

        private static void Write(string format, params object[] items)

        {

            Console.WriteLine(String.Format(format, items));

        }

        public static void Main(string[] args)

        {

            Run().Wait();

        }

        public static async Task Run()

        {

            BillingClient billingClient = await GetBillingClient();



            // Call the invoices service and expand the downloadURL, this call may take a while'

            // TODO: handle 404 invoice not found

            Write("Calling invoice service for all available invoices...");

            List<Invoice> allInvoices = billingClient.Invoices.List("downloadUrl").ToList();



            Write("{0} invoice(s) received.  Press ENTER to see them.", allInvoices.Count);

            Console.ReadLine();



            allInvoices.ForEach(inv => {

                Write("\tName: {0}", inv.Name);

                Write("\tDate: {0} to {1}", inv.InvoicePeriodStartDate, inv.InvoicePeriodEndDate);

                Write("\tPress ENTER to open PDF in browser");

                Console.ReadLine();

                OpenURL(inv.DownloadUrl.Url);

            });

            Write(Environment.NewLine);

        }

        public static async Task<BillingClient> GetBillingClient()

        {

            // Import config values from appsettings.json into billingClient, or throw an error if not found

            var builder = new ConfigurationBuilder()

                .SetBasePath(Directory.GetCurrentDirectory())

                .AddJsonFile("appsettings.json");



            Configuration = builder.Build();



            var tenantId = Configuration["TenantDomain"];

            var clientId = Configuration["ClientID"];

            var secret = Configuration["ClientSecret"];

            var subscriptionId = Configuration["SubscriptionID"];



            if(new List<string>{ tenantId, clientId, secret, subscriptionId }.Any(i => String.IsNullOrEmpty(i))) {

                throw new InvalidOperationException("Enter TenantDomain, ClientID, ClientSecret and SubscriptionId in appsettings.json");

            }

            else

            {

                // Build the service credentials and ARM client to call the billing API

                var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);

                var billingClient = new BillingClient(serviceCreds);

                billingClient.SubscriptionId = subscriptionId;

                return billingClient;

            }

        }

        public static void OpenURL(string url)

        {

            try

            {

                Process.Start(url);

            }

            catch

            {

                // https://github.com/dotnet/corefx/issues/10361

                if (RuntimeInformation.IsOSPlatform(OSPlatform.Windows))

                {

                    url = url.Replace("&", "^&");

                    Process.Start(new ProcessStartInfo("cmd", $"/c start {url}") { CreateNoWindow = true });

                }

                else if (RuntimeInformation.IsOSPlatform(OSPlatform.Linux))

                {

                    Process.Start("xdg-open", url);

                }

                else if (RuntimeInformation.IsOSPlatform(OSPlatform.OSX))

                {

                    Process.Start("open", url);

                }

                else

                {

                    throw;

                }

            }

        }

    }

}
##### 9. Access the Azure Resource Manager samples on Github. Search for a quickstart template that will allow you to set up a free SQL Database for a web application. This template has four files. One file is a markdown file. What is the type of the other three files?
.json
