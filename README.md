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

## Configure Jenkins Job
### 1. Create new Freestyle Project

### 2. Configure Project
#### Source Code Management > Repository URL
```
https://github.com/theandrewlane/jenkins-backup-script.git
```

#### Build periodically (Build Triggers > Build periodically) build daily at 3AM:
```
H 3 * * *
```

#### Build Step - Execute Shell (Run backup script)

```bash
./jenkins-backup.sh $JENKINS_HOME /path/to/backups_`date +"%Y%m%d%H%M%S"`.tar.gz
```

## Tips
### Rotate backup files
```bash
# keep backup with latest 30 days
find /path/to/backup_* -mtime +30 -delete
```

### Restore commands

```bash
sudo /etc/init.d/jenkins stop
cd /path/to/backup_dir
tar xzvf backup.tar.gz
sudo cp -R jenkins-backup/* /path/to/jenkins/
sudo chown jenkins:jenkins -R /path/to/jenkins/
sudo /etc/init.d/jenkins start
```
