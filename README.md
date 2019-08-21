# molecule_demo

## Getting Started

### Create the role 
```
$ molecule init role -r molecule_demo                                                                                                                             
--> Initializing new role molecule_demo...
Initialized role in /Users/sblack/Git/rhythmic/molecule_demo successfully.
```

## Example Playbook

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: molecule_demo, x: 42 }
