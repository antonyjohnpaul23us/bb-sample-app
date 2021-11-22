# bb-sample-app
## Just Containerizing a simple java war application and deploying it with tomcat helm chart.

Application of Interest: https://tomcat.apache.org/tomcat-8.0-doc/appdev/sample/


The Repository simply holds a sample application (sample.war) that is built as a docker container and deployed using bitnami/tomcat helm charts.

## Files of Interest
### Dockerfile:
This Dockerfile bundles the war on top of a tomcat base image

### Jenkinsfile:
- Since the Jenkins server and agent nodes are deployed in kubernetes, it is required to ensure the building of docker images. Instead of using 'docker build' in this scenario by exposing docker. sock which is a potential threat, kaniko has been chosen due to the benefits it has over docker during DinD operations.
- One the image is created with kaniko, it is pushed into docker registry (https://hub.docker.com/repository/docker/antonyjohnpaul23usgmail/bb-sample-app). The credentials for Dockerhub are scripted to auto-deploy as Kubernetes secrets while deploying Jenkins master into k8s.
- Helm has been used to deploy the docker container from dockerHub into the AKS instance.
- Git actions / scm are not configured since the input to the pipeline is a war file and not source code, but that can be done.
- Enhancements Suggested for a wholistic pipeline:
  - Creating a full fledged End to End Pipeline integrating build process from code, static code analysis ( and potential Build breaker thresholds)
  - Image scanning for potential Vulnerably
  - Test integration on deployed environment and closed loop feed back (Roll-back or Move Forward)
  - Automated Penetration Testing
  - Application promotion and deployment to Higher environments
  - Controlled Chaos engineering practices in as part of pipelines (Initially)

### helm_overwrite_tomcat_values.yaml
- This file is used to overwrite the default values of the Bitnami Tomcat Helm chart.
- The default chart is directly used form the global repository to deploy required k8s components



