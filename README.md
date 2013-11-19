### How CloudMinion works
CloudMinion is a set of tools for identifying unused OpenStack VMs, setting and managing expiration dates, shutting down and deleting expired VMs.
There are three main components: 
 - cm_agent
 - cm_manager
 - vmem.cgi

CloudMinion Agent (cm_agent.pl) runs on each OpenStack compute node periodically and collects usage statistics by guest-mounting each VM and then sends the data to the CloudMinion DB. The agent is a framework where users can create custom rules for identifying unused VMs. Currently, the default rule is to mark the VMs as unused if no login has occurred in the last 30 days.

CloudMinion Manager (cm_manager.pl) also runs periodically from a management node and it is processing the data about the VMs. If a VM is marked as unused, the manager sets an expiration date and sends a notification to the user. If the user takes no action, on the expiration date the VM will be shut down and the user notified again. The VM will be kept shut down for a number of days after which it will be deleted.  There are two configurable parameters which allow to set the number of days before the VM is shutdown and how long it needs to be kept stopped before deleted. If no action is taken by the user after the first notification, he/she will also receive reminders 2 days prior to the shutdown and 2 days prior to deleting the VM. Notifications are also sent when the VMs are shutdown and deleted. For VMs which belong to users with no email address, the notifications will be sent to a configurable email address.

The CM Manager can also be used to set expiration date to VMs with specific constraints. For example, VMs which have specific string in the hostname - there are many cases where users create VMs with 'test' in the hostname just to try the Cloud environment but never intend to use these VMs.

VM Expiration Manager (vmem.cgi) is a GUI which allows the users to manage the expiration date of their VMs. In the notification emails the users can also receive a link to the VM Expiration Manager where they can change the expiration date or delete the VM(s).
