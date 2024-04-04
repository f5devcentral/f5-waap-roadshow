Reviewing Signature-based Bot protection
########################################



1. Return to Web App & API Protection, in the left-hand navigation menu, click on App Firewall, under Manage.

2. On your App Firewall policy <namespace>-appfw, click the three dots in the Actions column and then click Manage Configuration.

3. Click Edit Configuration in the top right corner.

<screeshots>


4. Using the left-hand navigation, click Detection Settings. In the Detection Settings section, click the Signature-Based Bot Protection dropdown menu.

5. From the Signature-Based Bot Protection dropdown menu, select Custom

<screeshots>

6. In the expanded configuration window, observe the three Bot signature categories:

 - Malicious, 
 - Suspicious, 
 - Good. 
 
 Also observe the actions **Block**, **Ignore**, and **Report** which can be reviewed by selecting one of the dropdowns.

7. Click Cancel and Exit to leave this window.

<screeshots>

8. Open a terminal window or DOS prompt on your respective client and issue the following curl command.

   .. code-block:: none
      curl -v http://<namespace>.lab-sec.f5demos.com

Observe the User Agent and response content.


   .. note::
      curl is installed on Windows10+, and is available on most Linux or MAC platforms.


10. Return to the F5 Distributed Cloud Console, within Web App & API Protection in the left-hand navigation menu, under Overview click on Security

<screeshots>

11. Within the Security dashboard, scroll down to the Load Balancer section and click the configured Load Balancer <namespace>-lb.

<screeshots>

12. Select Security Analytics from the horizontal navigation.

13. Locate the most recent security event, which should be your curl request. Expand the security event as you have done in prior exercises to observe the “Suspicious” Bot reporting. Remember the setting for Suspicious Bot was set to Report from Step 6 above.



   .. note::
      You can review the steps of Lab1, Task 3, Step 8 to locate the information detail.

<screenshot>

14. Limits of the Signature based Bot Detection

Signature based Bot detection can be bypassed if you provide a pattern that does not match a bot signature. By presenting a less suspicious user-agent string, a threat actor can easily bypass the signature-based detection algorithm.

For example, if you repeat the curl request and with a less suspicious user-agent, you will skip signature-based bot detection. For example, if you run the following command:

   .. code-block:: none
      curl -v http://<namespace>.lab-sec.f5demos.com --user-agent "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.2.1 Safari/605.1.15"


This HTTP request will not show up in the Security Analytics however you will find it in Request logging.






