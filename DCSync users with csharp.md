# Finding DCSync Users with C# 

Tags: #dcsync #csharp 

Video Explanation -> [Finding DCSync users with C#](https://youtu.be/PVBtAwsc2CU)

In this blog post, we will be seeing how to fetch dcsync capable users across the forest using c#. 

Users with DCSync permissions can try to sync global catalog (GC) from domain controller. This is used to sync all changes occurred in one dc across all other dc's. We can use tools like secretsdump from impacket to dump user's password hashes if we have this DCSync permissions

We will be querying all objects for users that have this capability

## Importing namespaces

Import these Active Directory namepsaces to use classes in them

```csharp
using System.DirectoryServices;
using System.DirectoryServices.ActiveDirectory;
using System.DirectoryServices.AccountManagement;
using System.Security.AccessControl;
```

## Fetching all Domains in our Forest

First we need to fetch our current forest. We can do that using Forest class

```csharp
Forest f = Forest.GetCurrentForest()
```

Now we need all domains from our forest

```csharp
DomainCollection domains = f.GetDomains;
```

We can print all these domain names

```csharp
foreach(Domain d in domains)
{
	Console.WriteLine(d.Name);
}
```

## Getting DN of domain name

We need to convert domain name into DN
Eg: "tech69.local" to "DC=tech69,DC=local"

Idea is split string on basis of ".", we get two elements "tech69" and "local" and add "DC=" to each element and join them with ","
```csharp
string domainName = d.Name;

 string[] dcs = domainName.Split('.');
 for (int i = 0; i < dcs.Length; i++)
 {
  dcs[i] = "DC=" + dcs[i];
 }

 string domainDN = String.Join(",", dcs);
```

## GetDCSyncUsers Function

Let's write a function that takes domainDN as parameter and finds all users that have dcsync capability.

If we look at access control rights in this page -> [Microsoft GUID in ACE](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-adts/1522b774-6464-41a3-87a5-1e5633c3fbbb)

When we get the access control of an object, we receive GUID 
we need to lookup this guid on above  website to check for permissions.

I have already made a dictionary 
```csharp
Hashtable ht = new Hashtable();
ht.Add("DS-Replication-Get-Changes", "1131f6aa-9c07-11d1-f79f-00c04fc2dcd2");
ht.Add("DS-Replication-Get-Changes-All", "1131f6ad-9c07-11d1-f79f-00c04fc2dcd2");
ht.Add("DS-Replication-Get-Changes-In-Filtered-Set", "89e95b76-444d-4c62-991a-0facbeda640c");
ht.Add("DS-Replication-Manage-Topology", "1131f6ac-9c07-11d1-f79f-00c04fc2dcd2");
ht.Add("DS-Replication-Monitor-Topology", "f98340fb-7c5b-4cdb-a00b-2ebdfa115a96");
ht.Add("DS-Replication-Synchronize", "1131f6ab-9c07-11d1-f79f-00c04fc2dcd2");
```

Next we gonna bind to our domain and get all objects
```csharp

DirectoryEntry de = new DirectoryEntry("LDAP://" + domainDN);
DirectorySearcher ds = new DirectorySearcher();
     ds.SearchRoot = de;

foreach (SearchResult sr in ds.FindAll())
{
 try
  {
        DirectoryEntry temp = sr.GetDirectoryEntry();
        AuthorizationRuleCollection arc = temp.ObjectSecurity.GetAccessRules(true, true, typeof(NTAccount));

```

We are getting all the results using DirectorySearcher.FindAll() and in those results, we are getting the Access Rules of that object 

Now for each rule, we gonna match our object's ObjectType value which is GUID to our hashtable's values. If there is a match we gonna print that to console

```csharp
foreach (ActiveDirectoryAccessRule a in arc)
{
 if (domainDN.Contains(temp.Name.ToString()))
{

    foreach (DictionaryEntry d in ht)
        {
        if (d.Value.ToString() == a.ObjectType.ToString())
          {
    sw.WriteLine(a.IdentityReference);
   sw.WriteLine(d.Key.ToString());
        sw.WriteLine(a.ObjectType);
     //sw.WriteLine(a.AccessControlType);
   //sw.WriteLine(a.ActiveDirectoryRights);
	sw.WriteLine();
	}
     }
   }
 }
}
```

FULL CODE
```csharp

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Threading;
using System.Security.AccessControl;
using System.Security.Principal;
using System.DirectoryServices;
using System.DirectoryServices.ActiveDirectory;
using System.DirectoryServices.AccountManagement;
using System.Security.AccessControl;
using System.Collections;
using System.IO;

namespace ACL101
{
    class Program
    {

        public string GetDCSyncUsers(string domainDN)
        {
            string result;
            Console.WriteLine(domainDN);
            StringWriter sw = new StringWriter();
            sw.WriteLine("-------- DOMAIN: {0} ---------", domainDN);
            Hashtable ht = new Hashtable();
            ht.Add("DS-Replication-Get-Changes", "1131f6aa-9c07-11d1-f79f-00c04fc2dcd2");
            ht.Add("DS-Replication-Get-Changes-All", "1131f6ad-9c07-11d1-f79f-00c04fc2dcd2");
            ht.Add("DS-Replication-Get-Changes-In-Filtered-Set", "89e95b76-444d-4c62-991a-0facbeda640c");
            ht.Add("DS-Replication-Manage-Topology", "1131f6ac-9c07-11d1-f79f-00c04fc2dcd2");
            ht.Add("DS-Replication-Monitor-Topology", "f98340fb-7c5b-4cdb-a00b-2ebdfa115a96");
            ht.Add("DS-Replication-Synchronize", "1131f6ab-9c07-11d1-f79f-00c04fc2dcd2");

            DirectoryEntry de = new DirectoryEntry("LDAP://" + domainDN);
            DirectorySearcher ds = new DirectorySearcher();
            ds.SearchRoot = de;

            foreach (SearchResult sr in ds.FindAll())
            {
                try
                {
                    DirectoryEntry temp = sr.GetDirectoryEntry();
                    AuthorizationRuleCollection arc = temp.ObjectSecurity.GetAccessRules(true, true, typeof(NTAccount));

                    foreach (ActiveDirectoryAccessRule a in arc)
                    {
                        if (domainDN.Contains(temp.Name.ToString()))
                        {

                            foreach (DictionaryEntry d in ht)
                            {
                                if (d.Value.ToString() == a.ObjectType.ToString())
                                {
                                    sw.WriteLine(a.IdentityReference);
                                    sw.WriteLine(d.Key.ToString());
                                    sw.WriteLine(a.ObjectType);
                                    //sw.WriteLine(a.AccessControlType);
                                    //sw.WriteLine(a.ActiveDirectoryRights);

                                    sw.WriteLine();
                                }
                            }
                        }
                    }
                }
                catch { }
            }
            result = sw.ToString();
            return result;
        }
        static void Main(string[] args)
        {
            try
            {
                Forest f = Forest.GetCurrentForest();
                string cmd;
                DomainCollection domains = f.Domains;
                cmd = "";
                foreach (Domain d in domains)
                {
                    string domainName = d.Name;

                    string[] dcs = domainName.Split('.');
                    for (int i = 0; i < dcs.Length; i++)
                    {
                        dcs[i] = "DC=" + dcs[i];
                    }

                    string domainDN = String.Join(",", dcs);

                    Program p = new Program();
                    Thread dcsync = new Thread(() => { cmd += p.GetDCSyncUsers(domainDN); });
                    dcsync.Name = "get-dcsyncusers";
                    dcsync.Start();
                    dcsync.Join();

                }

                Console.WriteLine(cmd);
            }
            catch(Exception e)
            {
                Console.WriteLine(e.Message);
            }
```

