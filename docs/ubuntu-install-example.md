# <a name="InstallationExample"></a>Installation example using Ubuntu 22.04 LTS

Here's a step-by-step example of getting [Malcolm from GitHub]({{ site.github.repository_url }}/tree/{{ site.github.build_revision }}), configuring your system and your Malcolm instance, and running it on a system running Ubuntu Linux. Your mileage may vary depending on your individual system configuration, but this should be a good starting point.

The commands in this example should be executed as a non-root user.

You can use `git` to clone Malcolm into a local working copy, or you can download and extract the artifacts from the [latest release]({{ site.github.repository_url }}/releases).

To install Malcolm from the latest Malcolm release, browse to the [Malcolm releases page on GitHub]({{ site.github.repository_url }}/releases) and download at a minimum `install.py` and the `malcolm_YYYYMMDD_HHNNSS_xxxxxxx.tar.gz` file, then navigate to your downloads directory:
```
user@host:~$ cd Downloads/
user@host:~/Downloads$ ls
malcolm_common.py install.py  malcolm_20190611_095410_ce2d8de.tar.gz
```

If you are obtaining Malcolm using `git` instead, run the following command to clone Malcolm into a local working copy:
```
user@host:~$ git clone {{ site.github.repository_url }}
Cloning into 'Malcolm'...
remote: Enumerating objects: 443, done.
remote: Counting objects: 100% (443/443), done.
remote: Compressing objects: 100% (310/310), done.
remote: Total 443 (delta 81), reused 441 (delta 79), pack-reused 0
Receiving objects: 100% (443/443), 6.87 MiB | 18.86 MiB/s, done.
Resolving deltas: 100% (81/81), done.

user@host:~$ cd Malcolm/
```

Next, run the `install.py` script to configure your system. Replace `user` in this example with your local account username, and follow the prompts. Most questions have an acceptable default you can accept by pressing the `Enter` key. Depending on whether you are installing Malcolm from the release tarball or inside of a git working copy, the questions below will be slightly different, but for the most part are the same.
```
user@host:~/Malcolm$ sudo ./scripts/install.py
Installing required packages: ['apache2-utils', 'make', 'openssl', 'python3-dialog']

"docker info" failed, attempt to install Docker? (Y/n): y  

Attempt to install Docker using official repositories? (Y/n): y
Installing required packages: ['apt-transport-https', 'ca-certificates', 'curl', 'gnupg-agent', 'software-properties-common']
Installing docker packages: ['docker-ce', 'docker-ce-cli', 'containerd.io']
Installation of docker packages apparently succeeded

Add a non-root user to the "docker" group?: y   

Enter user account: user

Add another non-root user to the "docker" group?: n

"docker-compose version" failed, attempt to install docker-compose? (Y/n): y

Install docker-compose directly from docker github? (Y/n): y
Download and installation of docker-compose apparently succeeded

fs.file-max increases allowed maximum for file handles
fs.file-max= appears to be missing from /etc/sysctl.conf, append it? (Y/n): y

fs.inotify.max_user_watches increases allowed maximum for monitored files
fs.inotify.max_user_watches= appears to be missing from /etc/sysctl.conf, append it? (Y/n): y

fs.inotify.max_queued_events increases queue size for monitored files
fs.inotify.max_queued_events= appears to be missing from /etc/sysctl.conf, append it? (Y/n): y

fs.inotify.max_user_instances increases allowed maximum monitor file watchers
fs.inotify.max_user_instances= appears to be missing from /etc/sysctl.conf, append it? (Y/n): y

vm.max_map_count increases allowed maximum for memory segments
vm.max_map_count= appears to be missing from /etc/sysctl.conf, append it? (Y/n): y

net.core.somaxconn increases allowed maximum for socket connections
net.core.somaxconn= appears to be missing from /etc/sysctl.conf, append it? (Y/n): y

vm.swappiness adjusts the preference of the system to swap vs. drop runtime memory pages
vm.swappiness= appears to be missing from /etc/sysctl.conf, append it? (Y/n): y

vm.dirty_background_ratio defines the percentage of system memory fillable with "dirty" pages before flushing
vm.dirty_background_ratio= appears to be missing from /etc/sysctl.conf, append it? (Y/n): y

vm.dirty_ratio defines the maximum percentage of dirty system memory before committing everything
vm.dirty_ratio= appears to be missing from /etc/sysctl.conf, append it? (Y/n): y

/etc/security/limits.d/limits.conf increases the allowed maximums for file handles and memlocked segments
/etc/security/limits.d/limits.conf does not exist, create it? (Y/n): y
```

If you are configuring Malcolm from within a git working copy, `install.py` will now exit. Run `install.py` again like you did at the beginning of the example, only remove the `sudo` and add `--configure` to run `install.py` in "configuration only" mode.
```
user@host:~/Malcolm$ ./scripts/install.py --configure
```

Alternately, if you are configuring Malcolm from the release tarball you will be asked if you would like to extract the contents of the tarball and to specify the installation directory and `install.py` will continue:
```
Extract Malcolm runtime files from /home/user/Downloads/malcolm_20190611_095410_ce2d8de.tar.gz (Y/n): y

Enter installation path for Malcolm [/home/user/Downloads/malcolm]: /home/user/Malcolm
Malcolm runtime files extracted to /home/user/Malcolm
```

Now that any necessary system configuration changes have been made, the local Malcolm instance will be configured:
```
Malcolm processes will run as UID 1000 and GID 1000. Is this OK? (Y/n): y

Should Malcolm use and maintain its own OpenSearch instance? (Y/n): y

Forward Logstash logs to a secondary remote OpenSearch instance? (y/N): n

Setting 10g for OpenSearch and 3g for Logstash. Is this OK? (Y/n): y

Setting 3 workers for Logstash pipelines. Is this OK? (Y/n): y

Restart Malcolm upon system or Docker daemon restart? (y/N): y
1: no
2: on-failure
3: always
4: unless-stopped
Select Malcolm restart behavior (unless-stopped): 4

Require encrypted HTTPS connections? (Y/n): y

Will Malcolm be running behind another reverse proxy (Traefik, Caddy, etc.)? (y/N): n

Specify external Docker network name (or leave blank for default networking) (): 

Authenticate against Lightweight Directory Access Protocol (LDAP) server? (y/N): n

Store PCAP, log and index files locally under /home/user/Malcolm? (Y/n): y

Compress OpenSearch index snapshots? (y/N): n

Delete the oldest indices when the database exceeds a certain size? (y/N): n

Automatically analyze all PCAP files with Suricata? (Y/n): y

Download updated Suricata signatures periodically? (y/N): y

Automatically analyze all PCAP files with Zeek? (Y/n): y

Perform reverse DNS lookup locally for source and destination IP addresses in logs? (y/N): n

Perform hardware vendor OUI lookups for MAC addresses? (Y/n): y

Perform string randomness scoring on some fields? (Y/n): y

Expose OpenSearch port to external hosts? (y/N): n

Expose Logstash port to external hosts? (y/N): n

Expose Filebeat TCP port to external hosts? (y/N): y
1: json
2: raw
Select log format for messages sent to Filebeat TCP listener (json): 1

Source field to parse for messages sent to Filebeat TCP listener (message): message

Target field under which to store decoded JSON fields for messages sent to Filebeat TCP listener (miscbeat): miscbeat

Field to drop from events sent to Filebeat TCP listener (message): message

Tag to apply to messages sent to Filebeat TCP listener (_malcolm_beats): _malcolm_beats

Expose SFTP server (for PCAP upload) to external hosts? (y/N): n

Enable file extraction with Zeek? (y/N): y
1: none
2: known
3: mapped
4: all
5: interesting
Select file extraction behavior (none): 5
1: quarantined
2: all
3: none
Select file preservation behavior (quarantined): 1

Expose web interface for downloading preserved files? (y/N): y

Enter AES-256-CBC encryption password for downloaded preserved files (or leave blank for unencrypted): decryptme

Scan extracted files with ClamAV? (y/N): y

Scan extracted files with Yara? (y/N): y

Scan extracted PE files with Capa? (y/N): y

Lookup extracted file hashes with VirusTotal? (y/N): n

Download updated file scanner signatures periodically? (n/Y): y

Should Malcolm run and maintain an instance of NetBox, an infrastructure resource modeling tool? (y/N): n

Should Malcolm capture live network traffic to PCAP files for analysis with Arkime? (y/N): y

Capture packets using netsniff-ng? (Y/n): y   

Capture packets using tcpdump? (y/N): n

Should Arkime delete PCAP files based on available storage (see https://arkime.com/faq#pcap-deletion)? (y/N): y

Should Malcolm analyze live network traffic with Suricata? (y/N): y

Should Malcolm analyze live network traffic with Zeek? (y/N): y

Should Malcolm use "best guess" to identify potential OT/ICS traffic with Zeek? (y/N): n

Specify capture interface(s) (comma-separated): eth0

Capture filter (tcpdump-like filter expression; leave blank to capture all traffic) (): not port 5044 and not port 8005 and not port 9200

Disable capture interface hardware offloading and adjust ring buffer sizes? (y/N): n

Enable dark mode for OpenSearch Dashboards? (n/Y): y

Malcolm has been installed to /home/user/Malcolm. See README.md for more information.
Scripts for starting and stopping Malcolm and changing authentication-related settings can be found in /home/user/Malcolm/scripts.
```

At this point you should **reboot your computer** so that the new system settings can be applied. After rebooting, log back in and return to the directory to which Malcolm was installed (or to which the git working copy was cloned).

Now we need to [set up authentication](authsetup.md#AuthSetup) and generate some unique self-signed TLS certificates. You can replace `analyst` in this example with whatever username you wish to use to log in to the Malcolm web interface.
```
user@host:~/Malcolm$ ./scripts/auth_setup 

Store administrator username/password for local Malcolm access? (Y/n): y

Administrator username: analyst
analyst password:
analyst password (again):

Additional local accounts can be created at https://localhost:488/ when Malcolm is running

(Re)generate self-signed certificates for HTTPS access (Y/n): y 

(Re)generate self-signed certificates for a remote log forwarder (Y/n): y

Will Malcolm be using an existing remote primary or secondary OpenSearch instance? (y/N): n

Store username/password for email alert sender account? (y/N): n

(Re)generate internal passwords for NetBox (Y/n): y
```

For now, rather than [build Malcolm from scratch](development.md#Build), we'll pull images from [GitHub](https://github.com/orgs/idaholab/packages?repo_name=Malcolm):
```
user@host:~/Malcolm$ docker-compose pull
Pulling api               ... done
Pulling arkime            ... done
Pulling dashboards        ... done
Pulling dashboards-helper ... done
Pulling file-monitor      ... done
Pulling filebeat          ... done
Pulling freq              ... done
Pulling htadmin           ... done
Pulling logstash          ... done
Pulling name-map-ui       ... done
Pulling netbox            ... done
Pulling netbox-postgresql ... done
Pulling netbox-redis      ... done
Pulling nginx-proxy       ... done
Pulling opensearch        ... done
Pulling pcap-capture      ... done
Pulling pcap-monitor      ... done
Pulling suricata          ... done
Pulling upload            ... done
Pulling zeek              ... done

user@host:~/Malcolm$ docker images
REPOSITORY                                                     TAG               IMAGE ID       CREATED      SIZE
ghcr.io/idaholab/malcolm/api                                              23.04.0           xxxxxxxxxxxx   3 days ago   158MB
ghcr.io/idaholab/malcolm/arkime                                           23.04.0           xxxxxxxxxxxx   3 days ago   816MB
ghcr.io/idaholab/malcolm/dashboards                                       23.04.0           xxxxxxxxxxxx   3 days ago   1.02GB
ghcr.io/idaholab/malcolm/dashboards-helper                                23.04.0           xxxxxxxxxxxx   3 days ago   184MB
ghcr.io/idaholab/malcolm/file-monitor                                     23.04.0           xxxxxxxxxxxx   3 days ago   588MB
ghcr.io/idaholab/malcolm/file-upload                                      23.04.0           xxxxxxxxxxxx   3 days ago   259MB
ghcr.io/idaholab/malcolm/filebeat-oss                                     23.04.0           xxxxxxxxxxxx   3 days ago   624MB
ghcr.io/idaholab/malcolm/freq                                             23.04.0           xxxxxxxxxxxx   3 days ago   132MB
ghcr.io/idaholab/malcolm/htadmin                                          23.04.0           xxxxxxxxxxxx   3 days ago   242MB
ghcr.io/idaholab/malcolm/logstash-oss                                     23.04.0           xxxxxxxxxxxx   3 days ago   1.35GB
ghcr.io/idaholab/malcolm/name-map-ui                                      23.04.0           xxxxxxxxxxxx   3 days ago   143MB
ghcr.io/idaholab/malcolm/netbox                                           23.04.0           xxxxxxxxxxxx   3 days ago   1.01GB
ghcr.io/idaholab/malcolm/nginx-proxy                                      23.04.0           xxxxxxxxxxxx   3 days ago   121MB
ghcr.io/idaholab/malcolm/opensearch                                       23.04.0           xxxxxxxxxxxx   3 days ago   1.17GB
ghcr.io/idaholab/malcolm/pcap-capture                                     23.04.0           xxxxxxxxxxxx   3 days ago   121MB
ghcr.io/idaholab/malcolm/pcap-monitor                                     23.04.0           xxxxxxxxxxxx   3 days ago   213MB
ghcr.io/idaholab/malcolm/postgresql                                       23.04.0           xxxxxxxxxxxx   3 days ago   268MB
ghcr.io/idaholab/malcolm/redis                                            23.04.0           xxxxxxxxxxxx   3 days ago   34.2MB
ghcr.io/idaholab/malcolm/suricata                                         23.04.0           xxxxxxxxxxxx   3 days ago   278MB
ghcr.io/idaholab/malcolm/zeek                                             23.04.0           xxxxxxxxxxxx   3 days ago   1GB
```

Finally, we can start Malcolm. When Malcolm starts it will stream informational and debug messages to the console. If you wish, you can safely close the console or use `Ctrl+C` to stop these messages; Malcolm will continue running in the background.
```
user@host:~/Malcolm$ ./scripts/start
In a few minutes, Malcolm services will be accessible via the following URLs:
------------------------------------------------------------------------------
  - Arkime: https://localhost/
  - OpenSearch Dashboards: https://localhost/dashboards/
  - PCAP upload (web): https://localhost/upload/
  - PCAP upload (sftp): sftp://username@127.0.0.1:8022/files/
  - Host and subnet name mapping editor: https://localhost/name-map-ui/
  - NetBox: https://localhost/netbox/  
  - Account management: https://localhost:488/
  - Documentation: https://localhost/readme/

NAME                           COMMAND                  SERVICE              STATUS               PORTS
malcolm-api-1                  "/usr/local/bin/dock…"   api                  running (starting)   …
malcolm-arkime-1               "/usr/local/bin/dock…"   arkime               running (starting)   …
malcolm-dashboards-1           "/usr/local/bin/dock…"   dashboards           running (starting)   …
malcolm-dashboards-helper-1    "/usr/local/bin/dock…"   dashboards-helper    running (starting)   …
malcolm-file-monitor-1         "/usr/local/bin/dock…"   file-monitor         running (starting)   …
malcolm-filebeat-1             "/usr/local/bin/dock…"   filebeat             running (starting)   …
malcolm-freq-1                 "/usr/local/bin/dock…"   freq                 running (starting)   …
malcolm-htadmin-1              "/usr/local/bin/dock…"   htadmin              running (starting)   …
malcolm-logstash-1             "/usr/local/bin/dock…"   logstash             running (starting)   …
malcolm-name-map-ui-1          "/usr/local/bin/dock…"   name-map-ui          running (starting)   …
malcolm-netbox-1               "/usr/bin/tini -- /u…"   netbox               running (starting)   …
malcolm-netbox-postgres-1      "/usr/bin/docker-uid…"   netbox-postgres      running (starting)   …
malcolm-netbox-redis-1         "/sbin/tini -- /usr/…"   netbox-redis         running (starting)   …
malcolm-netbox-redis-cache-1   "/sbin/tini -- /usr/…"   netbox-redis-cache   running (starting)   …
malcolm-nginx-proxy-1          "/usr/local/bin/dock…"   nginx-proxy          running (starting)   …
malcolm-opensearch-1           "/usr/local/bin/dock…"   opensearch           running (starting)   …
malcolm-pcap-capture-1         "/usr/local/bin/dock…"   pcap-capture         running              …
malcolm-pcap-monitor-1         "/usr/local/bin/dock…"   pcap-monitor         running (starting)   …
malcolm-suricata-1             "/usr/local/bin/dock…"   suricata             running (starting)   …
malcolm-suricata-live-1        "/usr/local/bin/dock…"   suricata-live        running              …
malcolm-upload-1               "/usr/local/bin/dock…"   upload               running (starting)   …
malcolm-zeek-1                 "/usr/local/bin/dock…"   zeek                 running (starting)   …
malcolm-zeek-live-1            "/usr/local/bin/dock…"   zeek-live            running              …
…
```

It will take several minutes for all of Malcolm's components to start up. Logstash will take the longest, probably 3 to 5 minutes. You'll know Logstash is fully ready when you see Logstash spit out a bunch of starting up messages, ending with this:
```
…
malcolm-logstash-1  | [2022-07-27T20:27:52,056][INFO ][logstash.agent           ] Pipelines running {:count=>6, :running_pipelines=>[:"malcolm-input", :"malcolm-output", :"malcolm-beats", :"malcolm-suricata", :"malcolm-enrichment", :"malcolm-zeek"], :non_running_pipelines=>[]}
…
```

You can now open a web browser and navigate to one of the [Malcolm user interfaces](quickstart.md#UserInterfaceURLs).