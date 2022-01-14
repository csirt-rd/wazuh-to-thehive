_en_
## Wazuh and TheHive integration
This project integrates SIEM Wazuh and TheHive. Use the following instructions to configure:
 
```sh
$ cd /opt/
$ sudo git clone https://github.com/crow1011/wazuh2thehive.git
$ sudo /var/ossec/framework/python/bin/pip3 install -r /opt/wazuh2thehive/requirements.txt
$ sudo cp /opt/wazuh2thehive/custom-w2thive.py /var/ossec/integrations/custom-w2thive.py
$ sudo cp /opt/wazuh2thehive/custom-w2thive /var/ossec/integrations/custom-w2thive
$ sudo chmod 755 /var/ossec/integrations/custom-w2thive.py
$ sudo chmod 755 /var/ossec/integrations/custom-w2thive
$ sudo chown root:ossec /var/ossec/integrations/custom-w2thive.py
$ sudo chown root:ossec /var/ossec/integrations/custom-w2thive
$ sudo nano /var/ossec/etc/ossec.conf
```
insert the following snippet into the ossec_config block:
```xml
<integration>
    <name>custom-w2thive</name>
    <hook_url>http://localhost:9000</hook_url>
    <api_key>123456790</api_key>
    <alert_format>json</alert_format>
</integration>
```
lines description:

**name** - integration name(no need to change)

**hook_url** - TheHive host

**api\_key** - TheHive user's API key. You can generate the key on the user management page by logging in as administrator. For security, allow the api-user to create only an alert.

**alert\_format** - format that wazuh sends alert to the integrator(no need to change)

after configuration, apply the changes with this command:
```sh
/var/ossec/bin/ossec-control restart
```
Finally, check the /var/ossec/log/integrations.log file for errors. If there is not enough information from the errors, you can enable debug_mode by changing the line in the file custom-w2thive.py 
```python
debug_enabled = False
```
to 
```python
debug_enabled = True
```
If you receive too many events, you can set a severity threshold for events that will be send to TheHive. Set the value of the lvl_threshold variable in the file /var/ossec/integrations/custom-w2thive.py
```python
lvl_threshold = 0
```
Events with a severity level equal to or greater will be sent to TheHive. You can read more about event classification in Wazuh here: [wazuh-rules-classification](https://documentation.wazuh.com/3.12/user-manual/ruleset/rules-classification.html)


