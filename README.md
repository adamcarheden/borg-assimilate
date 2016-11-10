# borg-assimilate

> A quick-n-dirty script to add logging and monitoring (via cron MAILTO=) to [borg backup](https://borgbackup.readthedocs.io).

Note: You might find [borgmatic](https://github.com/witten/borgmatic) to be more useful and better maintained. But I think python pip is a PITA, so I wrote this instead.


## Installation and Usage
``` bash
# 'git' it
cd /usr/local
sudo git clone https://github.com/adamcarheden/borg-assimilate.git

# Put it in your path
sudo ln -s borg-assimilate/borg-assimilate bin/borg-assimilate
sudo mkdir -p etc/
sudo cp borg-assimilate/borg-assimilate.json etc/borg-assimilate.json
# You should probably edit /usr/local/etc/borg-assimilate.json
# However, if you just want to back up everything on your system to a repo
# at the path listed below, the example file is probably pretty close.

# Install deps 
yum install borgbackup perl-DateTime perl-JSON perl-File-Slurp perl-Getopt-Long
# That command works on Centos7 + EPEL
# Package managers, names and 3rd party repos will vary by OS/distribution

# Create a borg repo
mount /dev/disk/by-label/borg_repo /mnt/borg_repo
borg init /mnt/borg_repo
# Your backup media and where it's mounted is highly system-dependent, of course

# Make regular backups
cat <<EOF | sudo tee /etc/cron.d/borg-assimilate
MAILTO=me@mydomain.com

0 */4 * * * root /usr/local/sbin/borg-assimilate

EOF
```

You should now get log messages in syslog (usually /var/log/messages) about borg backups that happen every 4 hours and e-mails only if there was a problem.

This version is far from fully-featured, but it's a bit more useful than the [automation script](https://borgbackup.readthedocs.io) in the borg backup quick start guide. [PR's](https://help.github.com/articles/about-pull-requests/) welcome.

Don't forget to check out [borgmatic](https://github.com/witten/borgmatic) if you have the stomach for Python pip.



