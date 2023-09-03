## couchbase-ansible
```bash
- name: Couchbase Installation
hosts: couchbase_grup
become: true

tasks:
— name: Couchbase Installation | Vm Swappines 0
sysctl:
name: vm.swappiness
value: 0
state: present

- name: Couchbase Installation | Install Open JDK
apt:
name: openjdk-jdk
state: present
update_cache: yes

- name: Couchbase Installation | Copy installatiob file to /tmp
become: true
copy: src=/etc/ansible/files/couchbase-server-enterprise_surum-redhat8_amd64.deb dest=/tmp/

- name: Couchbase Installation | Install couchbase from deb file
become: true
apt:
deb: /tmp/couchbase-server-enterprise_surum-redhat8_amd64.deb

- name: Start couchbase server
become: true
service:
name: couchbase-server
enabled: yes
state: started

couchbase_os:
  firewalld: true
  disable_thp: true
  common_tools: true
  kernel_tunings: true
  user_limits: true

couchbase_cluster:
  name: Couchbase Installation | Demo
  rest_protocol: http
  port: 8091
  notifications: true
  index_storage: default
  default_services:
    - data
    - index
    - query
couchbase_memory_quotas:
  analytics: 2048
  data: 12000
  eventing: 256
  fts: 512
  index: 256

couchbase_security:
  admin_user: Admin
  admin_password: pass
  disable_http_ui: false
  disable_www_authenticate: false
  cluster_encryption_level: control

roles:
— customrole.disable-thp
— customrole.couchbase-exporter
```
