title:: Guides/IT/Enable Faillock on Fedora 36

- `authselect current`
	- Shows current authentication settings
- `authselect enable-feature with-faillock`
	- Enables faillock
- pam_faillock is added in system-auth and password-auth:
	- `grep -n faillock /etc/pam.d/system-auth`
	- `grep -n faillock /etc/pam.d/password-auth`
- The config file is under **/etc/security/faillock.conf**.
- To display failed login counts for a user:
	- `faillock --user test`
- Manually unlock account:
	- `faillock --user test --reset`