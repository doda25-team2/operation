### Q1.1 (Nov 10+) 

- Justin [2886282](https://github.com/doda25-team2/model-service/commit/2886282c4e4b8eb0b5e2b10a64d5bf10695e1dff), [7daa5d7](https://github.com/doda25-team2/app/commit/7daa5d79757fa2df24690c425b04231b083b7c0d), [model-service#1](https://github.com/doda25-team2/model-service/pull/1), [model-service#2](https://github.com/doda25-team2/model-service/pull/2) →
Split the original `smschecker` repository and distributed the codebase across our four repos. Prepared and migrated backend to `uv`. Prepared backend `Dockerfile` and updated `README`.

- Ayush [1d61139](https://github.com/doda25-team2/app/commit/1d61139ee145420d95ba14fe0402345a974a5ed1), [f0cc66d](https://github.com/doda25-team2/app/commit/f0cc66dadc5fc6888c76159a619a4e07aabd86be) →
Created the `Dockerfile` for the frontend and also made sure that all the files were correctly added to the app repository. Also did a rework on the `README` based on the containerization of the frontend.

- George [model-service#3](https://github.com/doda25-team2/model-service/pull/3):
Implemented multi-stage Dockerfile for model-service (823MB to 693MB ~16% reduction) using builder/runtime stage separation and selective file copying.
[model-service#4](https://github.com/doda25-team2/model-service/pull/4), [app#3](https://github.com/doda25-team2/app/pull/3):
Created GitHub Actions workflows for automated container image releases to GHCR in both model-service and app repositories, triggered by git version tags and tested with act CLI.