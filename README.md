## How CloudMinion works
CloudMinion is a set of tools for identifying unused OpenStack VMs, setting and managing expiration dates, shutting down and deleting expired VMs.
There three main components: 
 - cm_agent
 - cm_manager
 - vmem.cgi

Each OpenStack compute node runs periodically an agent (cm_agent.pl) which collects usage statistics by guest-mounting each VM and then sends the information to the CloudMinion DB. The agent is a framework where users can create custom rules for identifying unused VMs. Currently, the default rule is to mark the VMs as unused if no login has occurred in the last 30 days.

A CloudMinion manager (cm_manager.pl), which also runs periodically from a management node is processing that information. If a VM is marked as unused, the manager sets an expiration date and sends a notification to the user. The user also receives a link to the VM ExpirationManager (vmem.cgi) where he/she can change the expiration date or delete the VM.  If the user takes no action, on the expiration date the VM will be shut down and the user notified again. The VM will be kept shut down for a number of days after which it will be deleted.  There are two configurable parameters which allow to set the number of days before the VM is shutdown and how long it needs to be kept stopped before deleted. If no action is taken by the user after the first notification, he/she will also receive reminders 2 days prior to the shutdown and 2 days prior to deleting the VM. Notifications are also sent when the VMs are shutdown and deleted. For VMs which belong to users with no email address, the notifications will be sent to a configurable email address.

The CM Manager can also be used to set expiration date to VMs with specific constraints. For example, VMs which have specific string in the hostname - there are many cases where users create VMs with .test. in the hostname just to try the Cloud environment but never intend to use these VMs.

