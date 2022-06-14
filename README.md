<img src="https://avatars.githubusercontent.com/u/40179672" width="75">

[![hack.d Lawrence McDaniel](https://img.shields.io/badge/hack.d-Lawrence%20McDaniel-orange.svg)](https://lawrencemcdaniel.com)
[![discuss.overhang.io](https://img.shields.io/static/v1?logo=discourse&label=Forums&style=flat-square&color=ff0080&message=discuss.overhang.io)](https://discuss.overhang.io)
[![docs.tutor.overhang.io](https://img.shields.io/static/v1?logo=readthedocs&label=Documentation&style=flat-square&color=blue&message=docs.tutor.overhang.io)](https://docs.tutor.overhang.io)<br/>
[![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)

# tutor-plugin-enable-backup

Github Action to install and enable the Tutor plugin hastexo/tutor-contrib-backup

IMPORTANT: you must run tutor_build_hastexo_backup first. See the github README for additional AWS S3 configuration options in the event that you are not also installing hastexo/tutor-contrib-s3

## Usage:


```yaml
name: Example workflow

on: workflow_dispatch

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # required antecedent
      - uses: actions/checkout@v3.0.2

      # required antecedent
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1.6.1
        with:
          aws-access-key-id: ${{ secrets.THE_NAME_OF_YOUR_AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.THE_NAME_OF_YOUR_AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-2

      # install and configure tutor and kubectl
      - name: Configure Github workflow environment
        uses: openedx-actions/tutor-k8s-init@v0.0.13

      # Build the Backup container
      - name: Build the image and upload to AWS ECR
        uses: openedx-actions/tutor-plugin-build-backup@v0.1.2
        with:
          aws-ecr-repo: openedx_backup

      # This action.
      - name: Enable tutor plugin - Backup
        uses: openedx-actions/tutor-enable-plugin-backup@v0.0.9
        with:
          aws-ecr-uri: 123456789012.dkr.ecr.us-east-2.amazonaws.com/openedx_backup:latest
          namespace: openedx-prod
```
