### Week 1 & 2 (Nov 10 - Nov 23rd) 

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

- Preethika: [model-service#7](https://github.com/doda25-team2/model-service/pull/7), [model-service#6](https://github.com/doda25-team2/model-service/pull/6) Worked on F9 and F10 by implementing a GitHub Actions workflow for on-demand automated model training, versioning, and release, publishing trained artifacts via GitHub Releases for unauthenticated access, and refactoring model-service to eliminate hard-coded models, enabling dynamic model loading via volume mounts with fallback download on startup.

- Nick: I implemented F1 and F2. As the release workflow trigger only works when it is present on the main branch, I pushed directly to main for F2, and the commits can be viewed [here](https://github.com/doda25-team2/lib-version/compare/90cc6a99f3564f92f41765ec9012eb211c342319...29748c5dfa95a2c5e5c8b2e5595be181cc9df4e2). For F1, see [app#7](https://github.com/doda25-team2/app/pull/7).

### Week 3 (Nov 24 - Nov 30th)

- Miguel [bfcf924](https://github.com/doda25-team2/operation/pull/24) -> configured required kubernetes kernel modules across all nodes, added configuration file with `overlay` and `br_netfilter` modules, [a54f03c](https://github.com/doda25-team2/operation/pull/23), [da3d3df](https://github.com/doda25-team2/operation/pull/17) -> provided the base Vagrant environment for the kubernetes cluster, created the controlle rand worker bms and configured their settings.

- Ayush [operation#18](https://github.com/doda25-team2/operation/pull/18), [operation#20](https://github.com/doda25-team2/operation/pull/20), [operation#22](https://github.com/doda25-team2/operation/pull/22), [operation#25](https://github.com/doda25-team2/operation/pull/25) -> For Assignment 2, I worked on Step 3 in registering an Ansible provisioner in our Vagrantfile in which I created three playbooks which are general.yaml, ctrl.yaml and node.yaml. I also worked on Step 4 on registering the SSH key and created a ssh-keys directory to store other members ssh key. I worked on Step 5 and implemented the basic swapoff -a command and also the swap entry remove for the path etc/fstab in which another member added modifications in order to complete the step. Step 7 was also completed using the module sysctl in which kernel property net.ipv4.ip_forward was enabled in all cluster nodes and the same for the other properties. Step 8 was also done by using the blockinline module and jinja2 loop. Step 10 was also done installing the K8 tools that correctly are installed.

- George [#24](https://github.com/doda25-team2/operation/pull/24), [#27](https://github.com/doda25-team2/operation/pull/27), [8f1e1a6](https://github.com/doda25-team2/operation/commit/8f1e1a6a6afab03e071fcffc18ca47242c775a56) → Rebased and resolved conflicts on the Step 6 and Step 9/10 feature branches, verified Vagrant provisioning (`ctrl`, `node-1`), then fast-forward merged both PRs into `main`. Followed up by removing committed kubeconfig secrets and extending `.gitignore` so future `kubeadm init` artefacts stay out of version control.

- Nick [operation#34](https://github.com/doda25-team2/operation/pull/34) → Set up Vagrant on WSL using network mode Mirrored (took 12 hours and moving to Windows 11). Added generating random ed25519 SSH key and adding it to the `ctrl` VM during provisioning. Fixed nodes not being able to SSH into `ctrl` to generate a join token.

- Justin [#27](https://github.com/doda25-team2/operation/pull/27), [#32](https://github.com/doda25-team2/operation/pull/32/files)  , [#37](https://github.com/doda25-team2/operation/pull/37) → resolve merge conflicts and get step 11 and 12 ready for merge, refactor and streamline the `Vagrantfile` a few times - dropping the use of `ansible_local` in favor of host driven `ansible`.

- Preethika [operation#26](https://github.com/doda25-team2/operation/pull/26) -> Implemented fixes for step 8 in the assignment,
[operation#21](https://github.com/doda25-team2/operation/pull/21) -> Added changes for step 5, Worked on PR [operation#27](https://github.com/doda25-team2/operation/pull/27) -> worked on steps 13 to 18 for assignment 2. Implemented fixes for step 8 in the assignment (operation #21), and added the required changes for step 5 to ensure correctness and alignment with the specification. Worked on PR operation #27, completing steps 13–18 for Assignment 2, including configuration refinements and deployment-level updates. Additionally, validated the changes through local and cluster-level testing to confirm correct service startup and interaction.

### Week 4 (Dec 1st - Dec 7th)

- Ayush [operation#44](https://github.com/doda25-team2/operation/pull/44) [operation#45](https://github.com/doda25-team2/operation/pull/45) [operation#48](https://github.com/doda25-team2/operation/pull/48)-> Implemented the initial helm creation for the Helm Chart. Also added a small fix to make backend work with frontend ensuring ham and spam return the correct output. Also worked on the installation of the prometheus and the ServiceMonitor to bind the apps.
- Preethika [operation#45](https://github.com/doda25-team2/operation/pull/45) -> For Assignment 3, I used Helm to implement Kubernetes-based deployment, combining all services into a single Helm chart that allowed for one-command cluster setup with uniform configuration, service dependencies, and version control.
- Justin [operation#47](https://github.com/doda25-team2/operation/pull/47) → a very humble introduction of `grafana`. PR was created _before_ prometheus code was ready so it doesn't do much other than take one to a splash page.

- Miguel [0f3ab59](https://github.com/doda25-team2/operation/pull/46) -> extended helm chart to support application level configuration, added a `config map` and `secret` placeholder for SMTP credentials, also split up the tasks and created the issues on projects for A3

- George [operation#52](https://github.com/doda25-team2/operation/pull/52), [operation#53](https://github.com/doda25-team2/operation/pull/53) → Added a dedicated `finalization.yml` Ansible playbook that installs and wires MetalLB (downloaded manifests, webhook workaround, IP pool and L2Advertisement resources) so LoadBalancer services have a stable address range, then layered Step 21 on top with a Helm-managed NGINX ingress controller pinned to 192.168.56.95, ready checks, and the supporting SSH key material.

- Nick [operation#55](https://github.com/doda25-team2/operation/pull/55), [operation#56](https://github.com/doda25-team2/operation/pull/56) → Fixed Flannel to use the correct network interface (eth1) for communication between VMs. Added Istio to the Kubernetes cluster and configured it to use a MetalLB IP address. Also added Kubernetes Dashboard to the cluster.

### Week 5 (Dec 8th - Dec 14th)

- Ayush[operation#64](https://github.com/doda25-team2/operation/pull/64) [operation#70](https://github.com/doda25-team2/operation/pull/70)→ This week I worked on assignment 3 in which I implemented the Alerting in prometheus in which one PrometheusRule was used which was that an alert is sent when the service receives more than 15 requests per minute for two minutes. For assignment 4, I worked on implementing the extension proposal documentation.

- George [app#8](https://github.com/doda25-team2/app/pull/8), [operation#65](https://github.com/doda25-team2/operation/pull/65) → Implemented custom Prometheus metrics endpoint in the app repository with 5 manually-created metrics (Counters, Gauge, Histograms with labels). Fixed Prometheus scraping by correcting Content-Type headers and ServiceMonitor configuration. Modified AlertManager and PrometheusRule settings (alert thresholds, query windows, webhook routing). Cleaned up monitoring configuration and updated README with access URLs and testing instructions.

- Miguel [operation#68] (https://github.com/doda25-team2/operation/pull/68) → implemented Istio shadow launch for the model-service using traffic mirroring with VirtualService and DestinationRule.

- Nick [operation#71](https://github.com/doda25-team2/operation/pull/71), [operation#72](https://github.com/doda25-team2/operation/pull/72), [operation#73](https://github.com/doda25-team2/operation/pull/73) → Fixed deadlock when installing deployment Helm chart, removed overwritten DestinationRule, implemented A4 "Traffic Management" to route 90% of users to stable app deployment and 10% to canary deployment using Sticky Sessions.

- Justin [operation#66](https://github.com/doda25-team2/operation/pull/67) → Add and test `istio` installation to the `finalization.yml` playbook.
- Preethika [operation#69](https://github.com/doda25-team2/operation/pull/69) - Added clear deployment instructions and initial routing information to deployment.md, describing how the services are deployed and accessed within the cluster. Included details on service exposure, routing flow, and relevant configuration assumptions to support reproducibility.

### Week 6 (Dec 15th - Dec 21nd)

- George [operation#74](https://github.com/doda25-team2/operation/pull/74), [operation#75](https://github.com/doda25-team2/operation/pull/75) → Added shared VirtualBox folder mount to enable cross-VM persistent storage at /mnt/shared, satisfying A3 Excellent rubric requirement for hostPath volumes. Implemented environment variable support in docker-compose.yml with .env.example template and README documentation, allowing configurable image versions and ports for A1 Excellent rubric compliance.

- Nick [operation#76](https://github.com/doda25-team2/operation/pull/76) → Fixed Prometheus's bundled Grafana ingress, added custom dashboard with two timeseries panels.
- Justin - no work.
- Ayush - no work.
- Preethika - no work.
  
### Week _ (Dec 22nd - Dec 28th)

### Week _ (Dec 29th - Jan 4th)

### Week 7 (Jan 5th - Jan 11th)

- Ayush [operation#79](https://github.com/doda25-team2/operation/pull/79) → Within extension.md, more details were added and sources were used in order to explain further on which extensions were proposed and how they will help improve. Furthermore, an experiment design was explained and how they would be measured using metrics such as the amount of pull requests closed etc. I also made sure that PRs that have been left open for a while were carefully checked and closed.

- Nick [operation#82](https://github.com/doda25-team2/operation/pull/82) → Finalized the Grafana dashboard. Now displays 6 visualizations that use all custom metrics of the app, including timeseries, gauges and histograms with proper units.
  
- Justin [operation#83](https://github.com/doda25-team2/operation/pull/83) → In an effort to improve our documentation, I've added `mermaid` diagrams to `deployment`. These should show the components and how they interact together better.

- George [operation#84](https://github.com/doda25-team2/operation/pull/84), [operation#86](https://github.com/doda25-team2/operation/pull/86) → Make containerd config file idempotent by using the ansible handler pattern. Removed the unnecessary `operation/grafana/` since it is enabled via `kube-prometheus-stack`.
- Preethika [operation#77]https://github.com/doda25-team2/operation/pull/77) -> Built on the existing deployment documentation making it more extensive. Wrote more detailed explanations on canary testing, request flow and deployment hierarchy.

- Miguel [operation#78](https://github.com/doda25-team2/operation/pull/78), added finalization playblook to the vagrant provision.

### Week 8 (Jan 12th - Jan 18th)

- Ayush [operation#87](https://github.com/doda25-team2/operation/pull/87) [operation#88](https://github.com/doda25-team2/operation/pull/88)[operation#89](https://github.com/doda25-team2/operation/pull/89) → For the additional use cases, the rate limiting has been implemented in which this ensures that the anyone that send 10 requests or more will be blocked and this is shown with the status being 200 or 429. Also worked on the continuous -experimentation.md documentation and added the necessary information. Furthermore, a github workflow is added to automatically request review for any open PRs to determine whether this improves anything as part of the extension.

- Preethika [operation#92](https://github.com/doda25-team2/operation/pull/92) added Kubernetes Secrets and ConfigMaps for the model-service and app-service, fixed issues in existing Secrets to ensure correct configuration, aligned environment variables across services, and resolved startup problems caused by misconfigured or missing secret references.

- Justin [model-service#11](https://github.com/doda25-team2/model-service/pull/11) → An update of the version control used in the `model-service` repository. This PR fixes versioning inconsistencies between git tags and pyproject.toml by enforcing it as a single source of truth, introduces an explicit and documented versioning scheme for ML models with manual major/minor/patch bumps during training, and adds structured metadata to trained models (with optional compatibility flags in the Docker image) so changes and compatibility can be clearly understood. Also updated the README!

- George [operation#95](https://github.com/doda25-team2/operation/pull/95) → Implemented auto-generation of Ansible `inventory.cfg` file in Vagrantfile for A2 Excellent requirement. Inventory dynamically includes all active nodes (control + workers) with SSH key paths and scales automatically with WORKER_COUNT variable. Verified functionality with ansible-inventory and ansible ping on all nodes. Also made a change to the github action `autopr.yml`, which was facing a 403 error cause token had no writing permissions.

- Nick [operation#96](https://github.com/doda25-team2/operation/pull/96) → Added documentation for deployment of the Kubernetes cluster using Vagrant and Ansible with clear step-by-step instructions and troubleshooting tips. Also documented how to run the application Helm chart with Minikube for local development and testing.



### Week 9 (Jan 19th - Jan 25th)

- Justin [operation#100](https://github.com/doda25-team2/operation/pull/100), [operation#101](https://github.com/doda25-team2/operation/pull/101), [model-service#12](https://github.com/doda25-team2/model-service/pull/12) → #100 and #101 are related PRs which introduce a smoke-test workflow to the operations repository which is called whenever a PR is requested. While not a full integration test, this workflow does mimic the helm deployment aspects and shows that: at least, we can get to the splash page of the application. #12 may appear trivial, but it was a needed step during testing: the smoke-test was calling the latest tag, but this was broken (probably after I dumped old container artefacts which, for some reason, `latest` was still pointing too.)
- Ayush [operation#99](https://github.com/doda25-team2/operation/pull/98) -> A github workflow was created which is for the midweek CI checkpoint as part of the extension proposal in which this ensures that it runs at the end of every Wednesday, it checks if all team members opened a PR and if any is missing creates an issue. Furthermore, extensive work was done in order to finish the presentation due next week.
- Preethika [operation#106](https://github.com/doda25-team2/operation/pull/106) Worked on bug fixes to fix helm charts and make further enhancements to the deployments, [operation#111](https://github.com/doda25-team2/operation/pull/111) Worked on a bug in F10 of assignment where model was not being mounted. Implemented the changes in the docker compose file after localising the issue.

- Miguel [operation#97](https://github.com/doda25-team2/operation/pull/97) → Added some documentation on the countinous expermination with a general draft outlining an A/B testing experiment, including hypothesis, metrics, and decision process, Also extensive work was done on the presentation this week.

- Nick [operation#109](https://github.com/doda25-team2/operation/pull/109), [operation#112](https://github.com/doda25-team2/operation/pull/112) → Added Istio to Minikube deployment documentation and documented deploying the application using an existing Kubernetes cluster. Also added a new deployment for the canary version of the model-service and fixed the VirtualService to route all traffic to the canary app deployment to the canary model-service. Also worked on the presentation.

### Week 10 (Jan 26th, 27th)
- George [operation#78](https://github.com/doda25-team2/operation/pull/78), [operation#115](https://github.com/doda25-team2/operation/pull/115), [operation#116](https://github.com/doda25-team2/operation/pull/116)  → Fixed the bug in `Vagrantfile` that did not let finalazation.yaml to properly auto execute after vagrant up. Added a new dashboard (`assignment4-support.json`) showcasing the results of the canary vs stable experimentation, and updated the `continuous-experimentation.md`. Finally, updated the `README.md` to be up to date and added information to make the setting up process easier.
  
- Ayush [operation#117](https://github.com/doda25-team2/operation/pull/117) [operation#118](https://github.com/doda25-team2/operation/pull/118) [operation#119](https://github.com/doda25-team2/operation/pull/119)-> Extensively worked on the extension.md documentation in which most of it was rewritten for more clarity and further information was added for clarification and achieving all the criteria for the rubric. Within the extension.md, two extra sources have been added to further aid in dealing with the shortcoming. Within the experiment design, a hypothesis was created and further information added. Visualizations were added to enhance the documents and aid in explanation. For continuous-experimentation.md I ensured the document had clarity, added further explanation on the results interpretation and what they tell us and concluded why the hypothesis was accepted. I also discussed a limitation of the experiment and future work.
  
