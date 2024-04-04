Enabling F5 Distributed Cloud Bot Defense
#########################################


The following steps will enable you to deploy F5 Distributed Cloud Bot Defense and understand its implementation.

Preparation
-----------

1. Open another tab in your browser (Chrome shown), navigate to your application/Load Balancer configuration: http://<namespace>.lab-sec.f5demos.com.

2. Enable developer tools (Chrome shown (use F12)) and click on the Network tab.

3. Using the 3 bars/menu icon (top right), navigate to Access link.

4. In the resulting login screen use the following values to login and click Submit

Identity: user@f5.com

Token: password

<screenshot>

5. In the Developer window, find the POST to auth.php. You can also use the filter to find auth.php. Select the respective line as shown.

6. Select the Request tab in the payload window that appears and observe that you only see limited form POST data (identity, token, & submit).


Warning

   Make sure to logoff using the menu on the right of the web application you just accessed


Simulate an attack
------------------

Let’s explore how an attacker could perform credential stuffing attacks by using the curl command:

   .. code-block:: none
      curl -v http://<namespace>.lab-sec.f5demos.com/auth.php -H "Content-Type: application/x-www-form-urlencoded" --user-agent "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.2.1 Safari/605.1.15" --data-raw "identity=user%40f5.com&token=password&submit=Submit"

For this application, a successful logon will have a 302 response to the location ./data.php?page=data

If we try an invalid password (password2 instead of password) for the same request, we will also get a 302 response to the location ./index.php?page=access&err=02

With this knowledge, we could use curl to perform a credential stuffing attack and potentially avoid detection.


<Screenshots>


1. Return to the Load Balancer in the F5 Distributed Cloud Console, Manage > Load Balancer > HTTP Load Balancers and use the Action Dots and click Manage Configuration

2. Click Edit Configuration in the top right-hand corner.

<Screenshots>
<Screenshots>

Configure Bot Protection
------------------------

1. Click Bot Protection in the left-hand navigation.

2. In the Bot Protection section, use the drop down under Bot Defense and select Enable Bot Defense Standard.




3. In the new Bot Defense Policy section, click Configure.

4. In the new Protected App Endpoints window, under App Endpoint Type, click Configure.

5. In the new App Endpoint Type window, click Add Item.



6. In the Application Endpoint input the following values in the fields identified:

Name: auth-bot

HTTP Methods: POST

Protocol: BOTH

Path\Path Match: Prefix

Path\Prefix: /auth.php

Bot Traffic Mitigation\Select Bot Mitigation Action: Block


7. Scroll to the bottom and click **Apply**.

Click Apply on the App Endpoint Type window.

Observe the additional positioning options in the JavaScript Insertion section of the Protected App Endpoints window, then click Apply.






8. Observe that the Bot Defense Policy is now configured.

9. Click Other Settings in the left-hand navigation or scroll to the bottom on the HTTP Load Balancer screen, and click Save and Exit.




10. Repeat Task 2 Steps 1-6. Note you many need to close your browser and clear cookies

11. Observe now that there is additional telemetry being passed in the POST request. This telemetry will be used to determine if the connecting client is an Automated Bot.




Test the attack again
---------------------

Will F5 Distributed Cloud Bot Defense will prevent curl initiated logon requests and its ability to perform credential stuffing attacks. Let’s find out. Re-run our previously successful logon attempt:

   .. code-block:: none
      curl -v http://<namespace>.lab-sec.f5demos.com/auth.php -H "Content-Type: application/x-www-form-urlencoded" --user-agent "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.2.1 Safari/605.1.15" --data-raw "identity=user%40f5.com&token=password&submit=Submit"

As you can see, instead of signaling to a potential attacker that they have a good or bad password, we have prevented the would-be attacker from programmatically testing accounts.

F5 Distributed Cloud Bot Defense can protect against basic attacks performed with commands like curl to the most advanced attacks.

