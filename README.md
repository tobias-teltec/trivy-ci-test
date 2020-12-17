# trivy-ci-test
Pipeline to Trivy tests purposes

Have for stages:
 1 - Build Dockerfile in repo
 2 - Scan container vulnerabilities with Trivy
 3 - Push the image to registry
 4 - Publish the scan report with Junit
 
The Trivy scan is performed by the Trivy docker image.
The Trivy cache was mounted in the directory on Jenkins host and mounted by a volume on the docker.
The first Trivy command execute the scan and create the xml report with a junit template, the second Trivy command execute the scan with exit code 0 (ignore fail errors) for MEDIUM and LOW severities, and the third Trivy command execute the scanning for CRITICAL and HIGH vulnerabilities and fail in case of true.

The push on Docker Hub registry is performed with the Jenkins credentials to make the login.
