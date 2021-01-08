# new-relic-log-monitor

Here's a quick script that can be used to pull information from application logs and display them in your New Relic [Logs](https://docs.newrelic.com/docs/logs/log-management/get-started/get-started-log-management)  Manager.

This specific example pulls information from the `auth.log` file. This log can be particularly helpful to monitor as you'll have quick access into which users are running commands, specifically as root. You'll also be able to search to see if there have been any unreconginzed entry into your server by searching for the term "Accepted" once its all set up.

## Prerequisites
* Have a test file in the following location `/var/log/anothertest.log`
* Have `python 3` installed
* Have the New Relic [infrastructure agent](https://docs.newrelic.com/docs/infrastructure/install-infrastructure-agent) installed and configured

From the `/etc/newrelic-infra/logging.d` directory, create the python script:
```
sudo nano log-setup.py
```

In the text editor, enter the following:
```
#!/usr/bin/env python3

anothertest_log = open("anothertest.yml" , "w+")
AT = ["logs: \n","  - name: \"another_log\" \n","    file: /var/log/anothertest.log"]
anothertest_log.writelines(AT)
anothertest_log.close()

auth_log = open("auth-log.yml" , "w+")
AL = ["logs: \n","  - name: \"auth_log\" \n","    file: /var/log/auth.log"]
auth_log.writelines(AL)
auth_log.close()
```

After saving the file, you can run the following which will take care of generating the `yml` files:
```
sudo python3 log-setup.py
```


Now, you can login to your New Relic account, visit the `Logs` tab, and within moments, your logs should begin generating. Feel free to build onto this script and add more log entries. A potential **use-case** for this is being able to automate the generation of log monitoring across a fleet of managed servers.
