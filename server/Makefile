# OpenSSH Server Maintenance Makefile

# The purpose of this makefile is to help with deployment of new SSHD configs
# Before implementing a new config, you really want to test it, otherwise
# You might inadvertantly lock yourself out of the host by accident if the
# daemon chokes on any changes made in the config file.
# There is a paired 'test' function in the the 'client' makefile to test
# the ssh daemon and new config before restarting the daemon with the new
# config.

# Here, I define the test config to be the normal config, that is, this
# makefile expects that you make changes to the in use one, but test it before
# restarting the production daemon to use the changes. However, you may
# wish to place the test config somewhere else. This is just my preference.

# Where SSH Confs are stored
SSHCONF = /etc/ssh

# Target Binary
SSHD = /usr/sbin/sshd

# Where the Config is
CONFIG = $(SSHCONF)/sshd_config

# Backup
BACKUP = $(SSHCONF)/sshd_config.bak

# Where test Config is
TESTCONFIG = $(SSHCONF)/sshd_config

# Debug Verbosity (Max 3) during test action
DEBUG = -d

# Port to test config on
TESTPORT = 9022

# Group allowed to use passwords
PWGROUP = pwusers

# Create group that allows users in it to use passwords (vs. keys)
# The intent here is that, once an authorized_keys file is detected in
# the user's home directory, they are removed from this group.
createpwgrp:
	addgroup $(PWGROUP)

# Place Testconfig over config, assuming they are not the same
place:
	@[ ! $(TESTCONFIG) = $(CONFIG) ] && cp -f $(TESTCONFIG) $(CONFIG) || return 0

# Backup current config
backup: $(CONFIG)
	@cp -f $(CONFIG) $(BACKUP)

# Restore Backup config
restore: $(BACKUP)
	@[ -f $(BACKUP) ] && cp -f $(BACKUP) $(CONFIG)

# Restart SSH Daemon
restart:
	@systemctl restart ssh

# Test Action, Run SSHD with test settings
test:
	# Starts daemon interactively on the supplied port, in debug mode
	# with errors printed to stderr instead of syslog, using the
	# supplied test config file.
	@printf "Basic syntax checking\n"
	@$(SSHD) -t -f $(TESTCONFIG)
	@printf "Config syntax test seems to have succeeded... moving on...\n"
	@printf "Once the daemon is active, you will want to run the client 'test' makefile\n"
	@printf "When done, press CTRL-C to terminate the test daemon\n"
	# @$(SSHD) -o "ListenAddress 127.0.0.1" -e -D $(DEBUG) -p $(TESTPORT) -f $(TESTCONFIG)
	# Return 0 added to force action completion even if there is an error.
	@$(SSHD) -e -D $(DEBUG) -p $(TESTPORT) -f $(TESTCONFIG) || return 0

actions:
	@printf "This Makefile must be run with root/sudo to work\n"
	@printf "place\tCopy testconfig over config file\n"
	@printf "backup\tBackup current sshd_config\n"
	@printf "restore\tRestore a backed up sshd_config\n"
	@printf "restart\tRestart ssh daemon with config/new-config\n"
	@printf "test\tStart interactive ssh daemon on test port for a trial run\n"

