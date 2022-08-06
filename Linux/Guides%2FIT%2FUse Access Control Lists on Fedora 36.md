title:: Guides/IT/Use Access Control Lists on Fedora 36

- Taken from:
	- https://www.server-world.info/en/note?os=CentOS_Stream_8&p=acl
- ## Installation
	- `# dnf -y install acl`
- ## Setting ACLs
	- To give user `cent` read permissions:
		- `setfacl -m u:cent:r test.txt`
	- Confirming settings:
		- `getfacl test.txt`
	- Setting ACL to a directory recursively:
		- `setfacl -m -R u:cent:r ./testdir`
	- Set by group:
		- `setfacl -m g:security:rw ./testfile.txt`
	- Remove an ACL:
		- `setfacl -b test.txt`
	- Remove only for user `cent`:
		- `setfacl -x u:cent test.txt`