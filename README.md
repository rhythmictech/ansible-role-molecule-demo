# molecule_demo
A "role" to demonstrate the power of [Ansible Molecule](https://molecule.readthedocs.io/en/stable/)


## About 
Molecule is pretty rad, so rad that [Jeff Geerling is adopting it](https://www.jeffgeerling.com/blog/2018/testing-your-ansible-roles-molecule). 
It helps you develop Ansible roles by providing out of the box:
- linting with `yamlint`
- syntax checking with `ansible-lint`
- testing rigs on `docker`, `vagrant`, `ec2`, and friends
- idempotence tests
- side effect tests
- a fuller head of hair

## Getting Started

### Requirements
The main requirements are documented in `requirements.txt` but this demo assumes you have a few other things installed including
- docker
- vagrant
- aws cli 

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