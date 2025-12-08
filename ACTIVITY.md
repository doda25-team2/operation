### Q1.1 (Nov 10 - Nov 21th) 

- Justin [2886282](https://github.com/doda25-team2/model-service/commit/2886282c4e4b8eb0b5e2b10a64d5bf10695e1dff), [7daa5d7](https://github.com/doda25-team2/app/commit/7daa5d79757fa2df24690c425b04231b083b7c0d), [model-service#1](https://github.com/doda25-team2/model-service/pull/1), [model-service#2](https://github.com/doda25-team2/model-service/pull/2), [app#5](https://github.com/doda25-team2/app/pull/5) →
Split the original `smschecker` repository and distributed the codebase across our four repos. Prepared and migrated backend to `uv`. Prepared backend `Dockerfile` and updated `README`. Modified `docker-image-build` actions to support multi-platform builds and more granular semantic tagging.

- Ayush [1d61139](https://github.com/doda25-team2/app/commit/1d61139ee145420d95ba14fe0402345a974a5ed1), [f0cc66d](https://github.com/doda25-team2/app/commit/f0cc66dadc5fc6888c76159a619a4e07aabd86be), [app#1](https://github.com/doda25-team2/app/pull/1), [app#2](https://github.com/doda25-team2/app/pull/2) [lib-version#1](https://github.com/doda25-team2/lib-version/pull/1)→
Created the `Dockerfile` for the frontend and also made sure that all the files were correctly added to the app repository. Also did a rework on the `README` based on the containerization of the frontend. Also worked on automating the creation of the stable and prerelease. After the stable release was done it was automatically bumped to the next -SNAPSHOT.

- George [model-service#3](https://github.com/doda25-team2/model-service/pull/3):
Implemented multi-stage Dockerfile for model-service (823MB to 693MB ~16% reduction) using builder/runtime stage separation and selective file copying.
[model-service#4](https://github.com/doda25-team2/model-service/pull/4), [app#3](https://github.com/doda25-team2/app/pull/3):
Created GitHub Actions workflows for automated container image releases to GHCR in both model-service and app repositories, triggered by git version tags and tested with act CLI.


- Miguel [02b7a6b](https://github.com/doda25-team2/model-service/commit/02b7a6b839b5ddc210a2d70867e860a4297fe384), In the model-service repository: refactored the code to remove hard-coded model and preprocessor paths, introduced environment variables and default download URLs from GitHub Releases. 
[800cfab](https://github.com/doda25-team2/app/commit/800cfab0db8cb48fe87d65cdde467eff2d395d14), In the app repository: updated container configuration to support flexible environment variables, modified the controller to read the model service URL dynamically instead of using a fixed host.
[80428da](https://github.com/doda25-team2/model-service/commit/02b7a6b839b5ddc210a2d70867e860a4297fe384), In operation repository did a simple docker compose to launch the entire system from one base docker-compose.yml.

- Preethika: [model-service#7](https://github.com/doda25-team2/model-service/pull/7), [model-service#6](https://github.com/doda25-team2/model-service/pull/6)

- Nick: I implemented F1 and F2. As the release workflow trigger only works when it is present on the main branch, I pushed directly to main for F2, and the commits can be viewed [here](https://github.com/doda25-team2/lib-version/compare/90cc6a99f3564f92f41765ec9012eb211c342319...29748c5dfa95a2c5e5c8b2e5595be181cc9df4e2). For F1, see [app#7](https://github.com/doda25-team2/app/pull/7).

### Q1.2 (Nov 21 - Nov 28th)

- Miguel [bfcf924](https://github.com/doda25-team2/operation/pull/24) -> configured required kubernetes kernel modules across all nodes, added configuration file with `overlay` and `br_netfilter` modules, [a54f03c](https://github.com/doda25-team2/operation/pull/23), [da3d3df](https://github.com/doda25-team2/operation/pull/17) -> provided the base Vagrant environment for the kubernetes cluster, created the controlle rand worker bms and configured their settings.

- Ayush [operation#18](https://github.com/doda25-team2/operation/pull/18), [operation#20](https://github.com/doda25-team2/operation/pull/20), [operation#22](https://github.com/doda25-team2/operation/pull/22), [operation#25](https://github.com/doda25-team2/operation/pull/25) -> For Assignment 2, I worked on Step 3 in registering an Ansible provisioner in our Vagrantfile in which I created three playbooks which are general.yaml, ctrl.yaml and node.yaml. I also worked on Step 4 on registering the SSH key and created a ssh-keys directory to store other members ssh key. I worked on Step 5 and implemented the basic swapoff -a command and also the swap entry remove for the path etc/fstab in which another member added modifications in order to complete the step. Step 7 was also completed using the module sysctl in which kernel property net.ipv4.ip_forward was enabled in all cluster nodes and the same for the other properties. Step 8 was also done by using the blockinline module and jinja2 loop. Step 10 was also done installing the K8 tools that correctly are installed.

- George [#24](https://github.com/doda25-team2/operation/pull/24), [#27](https://github.com/doda25-team2/operation/pull/27), [8f1e1a6](https://github.com/doda25-team2/operation/commit/8f1e1a6a6afab03e071fcffc18ca47242c775a56) → Rebased and resolved conflicts on the Step 6 and Step 9/10 feature branches, verified Vagrant provisioning (`ctrl`, `node-1`), then fast-forward merged both PRs into `main`. Followed up by removing committed kubeconfig secrets and extending `.gitignore` so future `kubeadm init` artefacts stay out of version control.

- Nick [operation#34](https://github.com/doda25-team2/operation/pull/34) → Set up Vagrant on WSL using network mode Mirrored (took 12 hours and moving to Windows 11). Added generating random ed25519 SSH key and adding it to the `ctrl` VM during provisioning. Fixed nodes not being able to SSH into `ctrl` to generate a join token.

- Justin [#27](https://github.com/doda25-team2/operation/pull/27), [#32](https://github.com/doda25-team2/operation/pull/32/files)  , [#37](https://github.com/doda25-team2/operation/pull/37) → resolve merge conflicts and get step 11 and 12 ready for merge, refactor and streamline the `Vagrantfile` a few times - dropping the use of `ansible_local` in favor of host driven `ansible`.

- Preethika [operation#26](https://github.com/doda25-team2/operation/pull/26) -> Implemented fixes for step 8 in the assignment,
[operation#21](https://github.com/doda25-team2/operation/pull/21) -> Added changes for step 5, Worked on PR [operation#27](https://github.com/doda25-team2/operation/pull/27) -> worked on steps 13 to 18 for assignment 2.

### Q1.3 (Dec 1st - Dec 7th)

- Ayush [operation#44](https://github.com/doda25-team2/operation/pull/44) [operation#45](https://github.com/doda25-team2/operation/pull/45) [operation#48](https://github.com/doda25-team2/operation/pull/48)-> Implemented the initial helm creation for the Helm Chart. Also added a small fix to make backend work with frontend ensuring ham and spam return the correct output. Also worked on the installation of the prometheus and the ServiceMonitor to bind the apps.
- Preethika [operation#45](https://github.com/doda25-team2/operation/pull/45) -> implemented bringing up services with kubernetes(Helm) and made all the services to start up from one helm folder for Assignment 3.
- Justin [operation#47](https://github.com/doda25-team2/operation/pull/47) → a very humble introduction of `grafana`. PR was created _before_ prometheus code was ready so it doesn't do much other than take one to a splash page.

- Miguel [0f3ab59](https://github.com/doda25-team2/operation/pull/46) -> extended helm chart to support application level configuration, added a `config map` and `secret` placeholder for SMTP credentials, also split up the tasks and created the issues on projects for A3

- George [operation#52](https://github.com/doda25-team2/operation/pull/52), [operation#53](https://github.com/doda25-team2/operation/pull/53) → Added a dedicated `finalization.yml` Ansible playbook that installs and wires MetalLB (downloaded manifests, webhook workaround, IP pool and L2Advertisement resources) so LoadBalancer services have a stable address range, then layered Step 21 on top with a Helm-managed NGINX ingress controller pinned to 192.168.56.95, ready checks, and the supporting SSH key material.

- Nick [operation#55](https://github.com/doda25-team2/operation/pull/55), [operation#56](https://github.com/doda25-team2/operation/pull/56) → Fixed Flannel to use the correct network interface (eth1) for communication between VMs. Added Istio to the Kubernetes cluster and configured it to use a MetalLB IP address. Also added Kubernetes Dashboard to the cluster.
