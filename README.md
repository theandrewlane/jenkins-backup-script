# Jenkins backup script

Archive Jenkins settings and plugins

* `$JENKINS_HOME/*.xml`
* `$JENKINS_HOME/jobs/*/*.xml`
* `$JENKINS_HOME/nodes/*`
* `$JENKINS_HOME/plugins/*.jpi`
* `$JENKINS_HOME/secrets/*`
* `$JENKINS_HOME/users/*`

# Usage
```sh
./jenkins-backup.sh /path/to/jenkins_home archive.tar.gz

# add timestamp suffix
./jenkins-backup.sh /path/to/jenkins_home backup_`date +"%Y%m%d%H%M%S"`.tar.gz
```

# run with Jenkins Job
## 1. install Exclusive Execution Plugin
https://wiki.jenkins-ci.org/display/JENKINS/Exclusive+Execution+Plugin

## 2. New Job
![img](http://cdn-ak.f.st-hatena.com/images/fotolife/s/sue445/20131208/20131208001948.png)

## 3. Configure
### Source Code Management > Repository URL
```
https://github.com/theandrewlane/jenkins-backup-script.git
```

### Build Triggers > Build periodically

### Build Environment > Set exclusive Execution

### Build > Execute shell
ex.

```bash
./jenkins-backup.sh $JENKINS_HOME /path/to/backup_`date +"%Y%m%d%H%M%S"`.tar.gz
```

# Tips
## rotate backup files
```bash
# keep backup with latest 30 days
find /path/to/backup_* -mtime +30 -delete
```

## Restore commands
example

```bash
sudo /etc/init.d/jenkins stop
cd /path/to/backup_dir
tar xzvf backup.tar.gz
sudo cp -R jenkins-backup/* /path/to/jenkins/
sudo chown jenkins:jenkins -R /path/to/jenkins/
sudo /etc/init.d/jenkins start
```
