﻿=======================================
Field Notices
=======================================

.. Note::

 This field notice is provided as a service to our customers to proactively update them on major issues. This service is provided without any changes in our SLA. The information in this field notice will be updated as we learn more.



Field Notice 0006 (2019/09/30)
--------------------------------
 
**Controller Memory Leak Issue due to Account and Gateway Audit feature in 5.0 Release**


**Problem:**

We introduced a new feature in Release 5.0 to allow our customers to monitor Aviatrix Controller managed AWS Accounts and Gateways periodically as described at https://docs.aviatrix.com/HowTos/account_audit.html and https://docs.aviatrix.com/HowTos/gateway_audit.html. This audit feature ensures the Aviatrix System to function as designed. Unfortunately, the new software introduced a bug that causes a memory leak on the Controller. This software defect does not impact the Aviatrix gateway deployed in your network. Customers who are not running 5.0 at this time are not impacted - this issue only affects releases 5.0.2667 through 5.0.2773.
 
**Recommended Solution:**

We request that all customers running Aviatrix Software Releases 5.0.2667 through 5.0.2773 upgrade to 5.0.2782 or greater, as soon as possible, following the instructions at https://docs.aviatrix.com/HowTos/inline_upgrade.html. Our upgrades will not impact any tunnels or openvpn users and should be quite simple and fast for Aviatrix Controllers that are already on 5.0 release. If you need any support during this upgrade process, please open a new support ticket by sending a new email to support@aviatrix.com or by visiting https://aviatrix.zendesk.com. This upgrade will turn off the "AWS Account and Gateway Audit" feature.
 
**Alternate/Workaround Solution:**

For customers who cannot upgrade right away, we suggest that they work around by disabling the "AWS Account Audit" and “Gateway Audit” features following the instructions below.

  * Aviatrix Console > Accounts > Account Audit > AWS Account Status > Background Audit, toggle the setting to Disabled.
  * Aviatrix Console > Settings > Advanced > Gateway Audit > Background Audit, toggle the setting to Disabled.




Field Notice 0005 (2019/07/22)
--------------------------------

**Aviatrix VPN Client Connection Issues with SAML authentication**

Problem: Due to unforeseen circumstances, one certificate that we use in our Aviatrix VPN Client software became invalid, breaking the Aviatrix VPN Client functionality with SAML authentication method. Customers who are not using SAML for user authentication are not impacted and continue to use Aviatrix VPN Client or any OpenVPN compatible VPN clients such as Tunnelblick.

Recommended Solution:

  * Upgrade Controller to the latest release (4.7.501 or higher) using these `instructions <https://docs.aviatrix.com/HowTos/inline_upgrade.html>`_
  * Inform your remote vpn users to upgrade their Aviatrix VPN Clients (2.0.3 or higher) from `here <https://docs.aviatrix.com/Downloads/samlclient.html>`_. Please note that Mac/Safari and Win/IE are not supported

Alternate Solution: Only if you cannot upgrade Aviatrix Controller to 4.7.501 or higher.

  * Please have your remote vpn users upgrade their Aviatrix VPN Client (2.0.3 or higher) from `here <https://docs.aviatrix.com/Downloads/samlclient.html>`_
  * When they start a remote VPN session, after the authentication is successful they should see a message that looks like “Could not contact the VPN Client …“. Please ask your users to trust the certificate using the following instructions:
  
      * Mac/Safari: Click on Connect in the Aviatrix VPN Client, Sign into SAML/IdP, OK, Show Details, Visit the Website, Visit Website, Password, Update Settings
      * Mac/Chrome: Click on Connect in the Aviatrix VPN Client, Sign into SAML/IdP, OK, Advanced, Proceed to localhost.aviatrix.com
      * Win/Chrome: Click on Connect in the Aviatrix VPN Client, Sign into SAML/IdP, OK, Advanced, Proceed to localhost.aviatrix.com
      * Win/Firefox: Click on Connect in the Aviatrix VPN Client, Sign into SAML/IdP, OK, Advanced, Accept the Risk and Continue
      
  * If they continue to have issues, please have them clear their browser cache and restart their browser (`clear cache in Google <https://support.google.com/chrome/answer/2392709>`_, `clear cache in Firefox <https://support.mozilla.org/en-US/kb/delete-browsing-search-download-history-firefox>`_)



Field Notice 0004 (2019/2/6)
--------------------------------

**New Site2Cloud connections will not pass traffic for Aviatrix Systems running software prior to 4.0.691**
 
Problem: AWS introduced changes in VGW IPSEC VPN recently which broke VPN traffic passing. Existing VPN connections will not be affected. Customers who establish a new `Transit VPC to VGW <https://docs.aviatrix.com/HowTos/transitvpc_workflow.html#connect-the-transit-gw-to-aws-vgw>`_  connections will not pass traffic, even though they may be reported as being “UP”. 
 
Description: Aviatrix Software uses SHA256 to setup IPSEC VPN connections with AWS VGW. Due to changes made by AWS recently, we discovered that during the week of Feb 4th, 2019, new VPN Connections to an VGW IPSEC tunnel were not passing traffic. We have submitted a support ticket with AWS technical support team. AWS has recommended that we use SHA1 instead of SHA256 for the Phase 2 part of IPSec configuration. They have acknowledged the issue and are looking to address the problem. Meanwhile, the Aviatrix engineering team made updates based on AWS recommendation and has released a new build, 4.0.691, to address this issue.
 
Solution: Customers running into this issue are requested to upgrade their Aviatrix system to 4.0.691 or later using the instructions `here <https://docs.aviatrix.com/HowTos/inline_upgrade.html#how-to-upgrade-software>`__. After upgrading, follow these directions:

  * For a new connection to VGW in Transit Network scenario, customers should login to the Controller, go to Transit Network -> Setup and go to `Step 8 <https://docs.aviatrix.com/HowTos/transitvpc_workflow.html#remove-transit-gw-to-vgw-connection>`_ to disconnect the VGW and reconnect again via `Step 3 <https://docs.aviatrix.com/HowTos/transitvpc_workflow.html#connect-the-transit-gw-to-aws-vgw>`_.
  * For a standalone Site2Cloud connection, rebuild the connection through Controller GUI/Site2Cloud and avoid using SHA256 for Phase 2.
 
Please test your network connections and if you continue to face further issues after going through the above steps, please open a ticket by sending an email to support@aviatrix.com 

========================================================================================

Field Notice 0003 (2018/12/1)
--------------------------------
**TGW Orchestrator customers: Incorrect route advertisements from Aviatrix gateway to on-premise networks affecting 4.0 releases prior to 4.0.590 for TGW Hybrid Connection**

Problem:
If you use the Aviatrix TGW Orchestrator and `build a hybrid connection using Step 4, 5 and 6 <https://docs.aviatrix.com/HowTos/tgw_plan.html#optional-setup-aviatrix-transit-gw>`_, the Aviatrix Transit gateway always advertises 10.0.0.0/8, 172.16.0.0/12 and 192.168.0.0/16 to on-prem.  This could affect the on-premise networks if the on-prem routers also advertise any of the three routes. 

Description:
Aviatrix transit gateways use BGP to summarize and propagate the network routes. Due to an unexpected software change, software releases from 4.0.368 to 4.0.589 advertises 10.0.0.0/8, 172.16.0.0/12 and 192.168.0.0/16 routes to on-prem which affects the on-prem network if the on-prem routers also advertise any of the three routes. This issue has been fixed in 4.0.590 and all customers who have deployed TGW are advised to upgrade to 4.0.590 or later, immediately.

Solution:
Customers deploying TGW are requested to upgrade to 4.0.590 or later. Please follow the instructions `here <https://docs.aviatrix.com/HowTos/inline_upgrade.html#how-to-upgrade-software>`__ to perform the software upgrade. After upgrading to 4.0.590 or later, please go to TGW Orchestrator > Plan > Step 7 to detach Aviatrix Transit GW from TGW and re-attach Aviatrix Transit GW to TGW in Step 6.

Note: this issue does not affect customers who are not deploying TGW Orchestrator. But if you have plans to deploy, we advise you to upgrade to the latest software. 

========================================================================================

Field Notice 0002 (2018/10/19)
----------------------------------
**Route update propagation inconsistency from on-premise networks affecting 3.5 releases prior to 3.5.362 for Transit Network**

Problem:
BGP Route propagation could fail intermittently from on-premise networks to cloud networks in Transit Network.

Description:
Aviatrix controllers and gateways use BGP to summarize and propagate the network routes. Due to an unexpected software change, 3.5 releases prior to 3.5.362 are affected and cannot forward routes in certain scenarios. This issue has been addressed in 3.5.362 and all customers who have deployed Transit Network and are running any 3.5 release prior to 3.5.362 are advised to upgrade to 3.5.362 or later, immediately. Customers who are running software versions prior to 3.5 are not impacted by this issue.

Solution:
Customers deploying Transit Network are requested to upgrade to 3.5.362 or later, if they are running any 3.5.(<362) release. Please follow the instructions `here <https://docs.aviatrix.com/HowTos/inline_upgrade.html#how-to-upgrade-software>`__ to perform the software upgrade.

Support:
For further information or to open a support ticket, please visit https://www.aviatrix.com/support/.

========================================================================================

Field Notice 0001 (2018/10/19)
-----------------------------------------------------------------------------
**SSL UserVPN with SAML function might fail with Chrome v70**
 
Problem
Remote users connecting via SSL UserVPN functionality authenticated through SAML cannot establish session.
 
Description
Aviatrix controllers and gateways provide SSL UserVPN service with authentication through SAML as described in `this doc <https://docs.aviatrix.com/HowTos/VPN_SAML.html>`_. Google Chrome v70 has altered the behavior of an element in HTML (they add an extra whitespace). This affects our product’s ability to interface with the SAML id providers and breaks the authentication process, resulting in remoteVPN users not being able to connect to your network. We have notified Google about this issue.
 
Solution
Customers deploying SSL UserVPN with SAML authentication are requested to upgrade to 3.5.362 or later, if they are running 3.3 or later release. Please follow the instructions `here <https://docs.aviatrix.com/HowTos/inline_upgrade.html#how-to-upgrade-software>`__ to perform the software upgrade. For software version prior to 3.3, please reach out to us at support@aviatrix.com. Another workaround to restore the service to your users is to use Firefox as their default browser.
  
Support:
For further information, or to open a support ticket, please visit https://www.aviatrix.com/support/.  
 
 
========================================================================================


OpenVPN is a registered trademark of OpenVPN Inc.


.. disqus::
