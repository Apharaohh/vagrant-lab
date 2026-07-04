## What This Is
A reproducible, infrastructure-as-code lab that demonstrates automated cluster bring-up using Vagrant, Ansible, and Rocky Linux. Three VMs boot from a single definition file and configure in parallel via a declarative playbook.

## Why It Matters
- **Repeatable:** Destroy and recreate identical infrastructure with one command.
- **Scalable:** Change one number to go from 3 nodes to 30.
- **Idempotent:** Run the playbook 100 times, the state is always the same.
- **Documented:** Every step is in the SOP; no magic, no hand-clicks.

## Quick Start
1. Read [SOP.md](SOP.md) for step-by-step instructions.
2. Run `vagrant up` to boot the nodes.
3. Run `ansible-playbook -i inventory.yml site.yml` to configure them.

## What I learned
- Vagrant: infrastructure definition and reproducibility
- Ansible: idempotent configuration management
- YAML: declarative syntax for infrastructure
- Multi-node orchestration: parallelism at scale
