# molecule_demo
A "role" to demonstrate the power of [Ansible Molecule](https://molecule.readthedocs.io/en/stable/)


## About 
Molecule is pretty rad, so rad that [Jeff Geerling is adopting it](https://www.jeffgeerling.com/blog/2018/testing-your-ansible-roles-molecule). 
It helps you develop Ansible roles by providing out of the box:
- linting with `yamlint`
- syntax checking with `ansible-lint`
- test "scenarios" on `docker`, `vagrant`, `ec2`, and friends
- infrastructure testing with `testinfra`
- idempotence tests
- side effect tests
- a fuller head of hair 

## Overview 
Here, we'll 
- Install Ansible Molecule 
- Create an [Ansible Role](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html)
- Run molecule's suite of tests
- Create new [scenarios](https://molecule.readthedocs.io/en/stable/getting-started.html#molecule-scenarios), Including 
    - testing in docker (the default)
    - testing in vagrant 
    - testing in ec2

## Getting Started

### Requirements
The main requirements are documented in `requirements.txt` but this demo assumes you have a few other things installed including
- `docker`
- `vagrant`
- `aws` cli 

Practice safe python. Create a `virtualenv`.
```
virtualenv venv --python=python3.7
```

Install the python requirements
```
pip install -r requirements.txt
```

### Create the role 
```
$ molecule init role -r molecule_demo                                                                                                                             
--> Initializing new role molecule_demo...
Initialized role in /Users/sblack/Git/rhythmic/molecule_demo successfully.
```

## Run ALL the tests 
```
$ molecule test                                                                                                                        
--> Validating schema /Users/sblack/Git/rhythmic/molecule_demo/molecule/default/molecule.yml.
Validation completed successfully.
--> Test matrix
    
└── default
    ├── lint
    ├── cleanup
    ├── destroy
    ├── dependency
    ├── syntax
    ├── create
    ├── prepare
    ├── converge
    ├── idempotence
    ├── side_effect
    ├── verify
    ├── cleanup
    └── destroy
    
--> Scenario: 'default'
--> Action: 'lint'
--> Executing Yamllint on files found in /Users/sblack/Git/rhythmic/molecule_demo/...
Lint completed successfully.
--> Executing Flake8 on files found in /Users/sblack/Git/rhythmic/molecule_demo/molecule/default/tests/...
Lint completed successfully.
--> Executing Ansible Lint on /Users/sblack/Git/rhythmic/molecule_demo/molecule/default/playbook.yml...
Lint completed successfully.
--> Scenario: 'default'
--> Action: 'cleanup'
Skipping, cleanup playbook not configured.
--> Scenario: 'default'
--> Action: 'destroy'
    
    PLAY [Destroy] *****************************************************************
    
    TASK [Destroy molecule instance(s)] ********************************************
    changed: [localhost] => (item=None)
    changed: [localhost]
    
    TASK [Wait for instance(s) deletion to complete] *******************************
    FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
    ok: [localhost] => (item=None)
    ok: [localhost]
    
    TASK [Delete docker network(s)] ************************************************
    
    PLAY RECAP *********************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
    
    
--> Scenario: 'default'
--> Action: 'dependency'
Skipping, missing the requirements file.
--> Scenario: 'default'
--> Action: 'syntax'
    
    playbook: /Users/sblack/Git/rhythmic/molecule_demo/molecule/default/playbook.yml
    
--> Scenario: 'default'
--> Action: 'create'
[DEPRECATION WARNING]: docker_image_facts is kept for backwards compatibility 
but usage is discouraged. The module documentation details page may explain 
more about this rationale.. This feature will be removed in a future release. 
Deprecation warnings can be disabled by setting deprecation_warnings=False in 
ansible.cfg.
    
    PLAY [Create] ******************************************************************
    
    TASK [Log into a Docker registry] **********************************************
    skipping: [localhost] => (item=None) 
    
    TASK [Create Dockerfiles from image names] *************************************
    changed: [localhost] => (item=None)
    changed: [localhost]
    
    TASK [Discover local Docker images] ********************************************
    ok: [localhost] => (item=None)
    ok: [localhost]
    
    TASK [Build an Ansible compatible image] ***************************************
    ok: [localhost] => (item=None)
    ok: [localhost]
    
    TASK [Create docker network(s)] ************************************************
    
    TASK [Determine the CMD directives] ********************************************
    ok: [localhost] => (item=None)
    ok: [localhost]
    
    TASK [Create molecule instance(s)] *********************************************
    changed: [localhost] => (item=None)
    changed: [localhost]
    
    TASK [Wait for instance(s) creation to complete] *******************************
    FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
    changed: [localhost] => (item=None)
    changed: [localhost]
    
    PLAY RECAP *********************************************************************
    localhost                  : ok=6    changed=3    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
    
    
--> Scenario: 'default'
--> Action: 'prepare'
Skipping, prepare playbook not configured.
--> Scenario: 'default'
--> Action: 'converge'
    
    PLAY [Converge] ****************************************************************
    
    TASK [Gathering Facts] *********************************************************
    ok: [instance]
    
    TASK [molecule_demo : echo hello world] ****************************************
    ok: [instance] => {
        "msg": "hello world"
    }
    
    PLAY RECAP *********************************************************************
    instance                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    
    
--> Scenario: 'default'
--> Action: 'idempotence'
Idempotence completed successfully.
--> Scenario: 'default'
--> Action: 'side_effect'
Skipping, side effect playbook not configured.
--> Scenario: 'default'
--> Action: 'verify'
--> Executing Testinfra tests found in /Users/sblack/Git/rhythmic/molecule_demo/molecule/default/tests/...
    ============================= test session starts ==============================
    platform darwin -- Python 3.7.4, pytest-5.0.1, py-1.8.0, pluggy-0.12.0
    rootdir: /Users/sblack/Git/rhythmic/molecule_demo/molecule/default
    plugins: testinfra-3.0.5
collected 1 item                                                               
    
    tests/test_default.py .                                                  [100%]
    
    =========================== 1 passed in 5.35 seconds ===========================
Verifier completed successfully.
--> Scenario: 'default'
--> Action: 'cleanup'
Skipping, cleanup playbook not configured.
--> Scenario: 'default'
--> Action: 'destroy'
    
    PLAY [Destroy] *****************************************************************
    
    TASK [Destroy molecule instance(s)] ********************************************
    changed: [localhost] => (item=None)
    changed: [localhost]
    
    TASK [Wait for instance(s) deletion to complete] *******************************
    FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
    changed: [localhost] => (item=None)
    changed: [localhost]
    
    TASK [Delete docker network(s)] ************************************************
    
    PLAY RECAP *********************************************************************
    localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
    
```


### Run the role against our default test rig

```
$ molecule converge                                                                                                                        
--> Validating schema /Users/sblack/Git/rhythmic/molecule_demo/molecule/default/molecule.yml.
Validation completed successfully.
--> Test matrix
    
└── default
    ├── dependency
    ├── create
    ├── prepare
    └── converge
    
--> Scenario: 'default'
--> Action: 'dependency'
Skipping, missing the requirements file.
--> Scenario: 'default'
--> Action: 'create'
[DEPRECATION WARNING]: docker_image_facts is kept for backwards compatibility 
but usage is discouraged. The module documentation details page may explain 
more about this rationale.. This feature will be removed in a future release. 
Deprecation warnings can be disabled by setting deprecation_warnings=False in 
ansible.cfg.
    
    PLAY [Create] ******************************************************************
    
    TASK [Log into a Docker registry] **********************************************
    skipping: [localhost] => (item=None) 
    
    TASK [Create Dockerfiles from image names] *************************************
    changed: [localhost] => (item=None)
    changed: [localhost]
    
    TASK [Discover local Docker images] ********************************************
    ok: [localhost] => (item=None)
    ok: [localhost]
    
    TASK [Build an Ansible compatible image] ***************************************
    ok: [localhost] => (item=None)
    ok: [localhost]
    
    TASK [Create docker network(s)] ************************************************
    
    TASK [Determine the CMD directives] ********************************************
    ok: [localhost] => (item=None)
    ok: [localhost]
    
    TASK [Create molecule instance(s)] *********************************************
    changed: [localhost] => (item=None)
    changed: [localhost]
    
    TASK [Wait for instance(s) creation to complete] *******************************
    FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
    changed: [localhost] => (item=None)
    changed: [localhost]
    
    PLAY RECAP *********************************************************************
    localhost                  : ok=6    changed=3    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
    
    
--> Scenario: 'default'
--> Action: 'prepare'
Skipping, prepare playbook not configured.
--> Scenario: 'default'
--> Action: 'converge'
    
    PLAY [Converge] ****************************************************************
    
    TASK [Gathering Facts] *********************************************************
    ok: [instance]
    
    TASK [molecule_demo : echo hello world] ****************************************
    ok: [instance] => {
        "msg": "hello world"
    }
    
    PLAY RECAP *********************************************************************
    instance                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```


Now we can log in!
```
$ molecule login
--> Validating schema /Users/sblack/Git/rhythmic/molecule_demo/molecule/vagrant/molecule.yml.
Validation completed successfully.
--> Validating schema /Users/sblack/Git/rhythmic/molecule_demo/molecule/default/molecule.yml.
Validation completed successfully.
[root@instance /]# 
```

### Create a Vagrant Scenario and Run it!

This [scenario](https://molecule.readthedocs.io/en/stable/getting-started.html#molecule-scenarios)
is using the vagrant [driver]() 
and 


```
$ molecule init scenario -d vagrant -s vagrant                                                                                           
--> Initializing new scenario vagrant...
Initialized scenario in /Users/sblack/Git/rhythmic/molecule_demo/molecule/vagrant successfully.
```

Run it 
```
$ molecule converge -s vagrant
....
```

Log in
```
$ molecule login -s vagrant
--> Validating schema /Users/sblack/Git/rhythmic/molecule_demo/molecule/vagrant/molecule.yml.
Validation completed successfully.
--> Validating schema /Users/sblack/Git/rhythmic/molecule_demo/molecule/default/molecule.yml.
Validation completed successfully.
Warning: Permanently added '[127.0.0.1]:2222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-157-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

13 packages can be updated.
9 updates are security updates.

New release '18.04.2 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


Last login: Wed Aug 21 20:15:46 2019 from 10.0.2.2
vagrant@instance:~$ 
```


### Again with ec2 as the driver 

Create the Scenario
```
$ molecule init scenario -d ec2 -s ec2-scenario                                                                                        
- -> Initializing new scenario ec2-scenario...
Initialized scenario in /Users/sblack/Git/rhythmic/molecule_demo/molecule/ec2-scenario successfully.
```

Log in to AWS
```
$ aws_okta_login infraservices-admin  
```

* This is where I changed a few values in `molecule/ec2-scenario/molecule.yml` 
so that ansible would use the right image in the right subnet. The Ansible code used to spin up the ec2 instances is in `molecule/ec2-scenario/create.yml`.


Now we can run it!
```
$ molecule converge -s ec2-scenario                                                                                                        
--> Validating schema /Users/sblack/Git/rhythmic/molecule_demo/molecule/ec2-scenario/molecule.yml.
Validation completed successfully.
--> Validating schema /Users/sblack/Git/rhythmic/molecule_demo/molecule/vagrant/molecule.yml.
Validation completed successfully.
--> Validating schema /Users/sblack/Git/rhythmic/molecule_demo/molecule/default/molecule.yml.
Validation completed successfully.
--> Test matrix
    
└── ec2-scenario
    ├── dependency
    ├── create
    ├── prepare
    └── converge
    
--> Scenario: 'ec2-scenario'
--> Action: 'dependency'
Skipping, missing the requirements file.
--> Scenario: 'ec2-scenario'
--> Action: 'create'
Skipping, instances already created.
--> Scenario: 'ec2-scenario'
--> Action: 'prepare'
Skipping, instances already prepared.
--> Scenario: 'ec2-scenario'
--> Action: 'converge'
    
    PLAY [Converge] ****************************************************************
    
    TASK [Gathering Facts] *********************************************************
    ok: [instance]
    
    TASK [molecule_demo : echo hello world] ****************************************
    ok: [instance] => {
        "msg": "hello world"
    }
    
    PLAY RECAP *********************************************************************
    instance                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```