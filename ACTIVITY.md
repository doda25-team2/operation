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

- Miguel [bfcf924](https://github.com/doda25-team2/operation/pull/24), [a54f03c](https://github.com/doda25-team2/operation/pull/23), [da3d3df](https://github.com/doda25-team2/operation/pull/17)
