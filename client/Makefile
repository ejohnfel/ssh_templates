# OpenSSH Client Maintenance Makefile

# The purpose of this makefile is to help with deployment of new SSHD configs
# and client configs.

# Host to use (if not listening on 127.0.0.1 or 0.0.0.0, change to IP/DNS Name here
HOST = 127.0.0.1

# Port to test config on
TESTPORT = 9022

# Set perms on .ssh and subordinate files so ssh/sshd does not choke on them
fixperms:
	# Make .ssh if it doesn't exist
	@[ ! -d ~/.ssh ] && mkdir ~/.ssh
	# Set perms acceptable to sshd
	@chmod -R u=rwx,go-rwx ~/.ssh

# Deploy example user config
deploy-config:
	@cp config ~/.ssh/config
	@chmod u=rw,go-rwx,u-x ~/.ssh/config
	@nano ~/.ssh/config

# Test Action, attempt login on sshd test instance
test:
	@ssh ${LOGNAME}@$(HOST) -p $(TESTPORT)

actions:
	@printf "Most actions in this makefile operate on the owners SSH settings\n"
	@printf "test\tAttempt to login to test daemon\n"
	@printf "fixperms\tCreate and/or set proper permissons on .ssh folder\n"
	@printf "deploy-config\tCopy example ssh config to ~/.ssh and edit\n"
