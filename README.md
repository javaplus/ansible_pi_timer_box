# ansible_pi_timer_box

Before running scripts need to set up pi to connect to network

then add your ansible servers public key to the client with this command:

ssh-copy-id -i ~/.ssh/id_rsa.pub [userid]@[IP of new client]
```
EX: ssh-copy-id -i ~/.ssh/id_rsa.pub pi@192.168.1.117
```

Run the ssh-add command with no arguments to add the key so you don't have to keep typing password:
```
ssh-add 
```
If you see this error when running ssh-add command: *Could not open a connection to your authentication agent.*, then run this command:
```
eval $(ssh-agent)
```


Update the /etc/ansible/hosts file to add clients
Example:/etc/ansible/hosts file entry
```
192.168.1.117 ansible_connection=ssh ansible_ssh_user=pi

```

Run ansible recipes like this:

ansible-playbook adafruit.yml -v

the -v means verbose

Reboot all:

ansible all -a "/sbin/reboot" --become -f 5 

NOTE: Had issue with the adafruit.yml... the last step tries to run the setup.py script from Adafruit.
However, it seems that the setup.py from Adafruit is trying to download the setuptools-3.5.1.zip, but it get's redirected or some issue so the actual zip is never downloaded so you get an error like this:

```
Downloading https://pypi.python.org/packages/source/s/setuptools/setuptools-3.5.1.zip
Extracting in /tmp/tmplhdTN_
Traceback (most recent call last):
  File "setup.py", line 4, in <module>
    use_setuptools()
  File "/home/pi/workspace/Adafruit_Python_LED_Backpack/ez_setup.py", line 128, in use_setuptools
    return _do_download(version, download_base, to_dir, download_delay)
  File "/home/pi/workspace/Adafruit_Python_LED_Backpack/ez_setup.py", line 108, in _do_download
    _build_egg(egg, archive, to_dir)
  File "/home/pi/workspace/Adafruit_Python_LED_Backpack/ez_setup.py", line 57, in _build_egg
    with archive_context(archive_filename):
  File "/usr/lib/python2.7/contextlib.py", line 17, in __enter__
    return self.gen.next()
  File "/home/pi/workspace/Adafruit_Python_LED_Backpack/ez_setup.py", line 88, in archive_context
    with get_zip_class()(filename) as archive:
  File "/usr/lib/python2.7/zipfile.py", line 770, in __init__
    self._RealGetContents()
  File "/usr/lib/python2.7/zipfile.py", line 813, in _RealGetContents
    raise BadZipfile, "File is not a zip file"
zipfile.BadZipfile: File is not a zip file

```
This issue was documented on Adafruits GitHub repo here ["File is not a zip file"](https://github.com/adafruit/Adafruit_Python_SSD1306/issues/22)

The fix that worked from me was changing to the Adafruit project (**~/workspace/Adafruit_Python_LED_Backpack**)removing the bad setuptools-3.5.1.zip (which is really just an error html page) and then running:
```
sudo wget https://pypi.python.org/packages/source/s/setuptools/setuptools-3.5.1.zip

```
This will download the setuptools and then I manually ran the setup command:
```
 sudo python setup.py install
```

This allowed the setup to complete fine.  I'm not updating the script for now, because I'm hoping Adafruit will fix their setup.py script to handle the download of the setuptools better in the future.  If this doesn't happen real soon, then I'll add the manual downloading of the setuptools myself before running the setup.py command.


#Note environment variable
Some source Code has been updated to to pull server IP from environment variable, but ansible scripts have not been updated.
Need to add **pi_server_ip=10.0.0.1** to **/etc/environment** in order for mqtt_server.py, speech_server.py, and maybe a bomb py to work.

