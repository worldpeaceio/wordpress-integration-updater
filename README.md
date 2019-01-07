This docker image runs as a Kubernetes cron job on the [Gaia cluster](https://github.com/worldpeaceio/gaia/). The image is repsonsible for running automatic updates for the [WordPress Integration Docker Image](https://github.com/nateinaction/wordpress-integration/). When spun up, the image checks the current WordPress version in the integration docker and what's available from the WordPress API. If it detects an available update, the image will clone the repo, update the image, and publish to master.

When the updated Integration Docker image is published to master, a Travis CI job will build the image and verify it passes tests before pushing to Docker Hub.

The updator in `main.py` is a Github application and expects a private RSA key named `github_app_key.pem` to be mounted at `/secrets/github_app_key.pem`. A K8s secret can be created by running `kubectl create secret generic github-app-key-pem --from-file=./github_app_key.pem`: https://kubernetes.io/docs/concepts/configuration/secret/

This repo's master branch image is automatically built and published by Docker Hub.
