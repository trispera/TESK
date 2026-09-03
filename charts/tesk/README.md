# Deployment instructions for TESK

Helm chart and config to deploy the TESK service. Tested with Helm v3.0.0.

## Usage

First you must create a namespace in Kubernetes in which to deploy TESK. In development clusters such as Minikube it is fine to use the `default` namespace. The
command below creates all deployments in the context of this namespace. How
the namespace is created depends on the cluster, so it is not documented here.

To deploy the application:
 * modify [`values.yaml`](values.yaml)
 * If you are installing the FTP storage backend (and will not use .netrc file for FTP credentials) and/or OIDC client, create a `secrets.yaml` file. You need to fill up the `username` and `password` of the ftp account that will be potentially used to exchange I/O with a workflow manager such as cwl-tes. If you activated authentication (auth.mode == 'auth' in `values.yaml`), optionally you may activate the OICD client in the Swagger UI as well (you need to register the client by your OIDC provider). To do so, supply the `client_id` and `client_secret` values obtained during the client registration, otherwise the auth section must be removed.

 ```
 ftp:
   username: <username>
   password: <password>

 auth:
   client_id: <client_id>
   client_secret: <client_secret>
 ```
 
 * If you're using `s3` as your storage option, do not forget to add the necessary `config` and `credentials` files
 (see [here](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)) under a folder named
 **s3-config** (*charts/tesk/s3-config*).

 * If you are installing the FTP storage backend and want to use .netrc file for FTP credentials, place the .netrc in the `ftp` folder. There is a template in the folder.

 * Finally execute:

```bash
$ helm upgrade --install tesk-release . -f secrets.yaml -f values.yaml
```

then you can check it all went as expected:

```bash
$ helm list -n tesk
NAME	        NAMESPACE  REVISION	UPDATED                                 	STATUS  	CHART     	APP VERSION
tesk-release	tesk	     16      	2020-05-14 11:38:45.521995325 +0300 EEST	deployed	tesk-0.1.0	dev
```

The first time running this command, the chart will be installed. Afterwards, it will apply any change in the chart.

*IMPORTANT:* In kubernetes, if you want to install TESK in a namespace different than `default`, it must be specified by using `-n <namespace>`, so the final command would be something like this:

```bash
$ helm upgrade -n tesk --install tesk-release . -f secrets.yaml -f values.yaml
```
*Note*: If you're running Helm 3, you might need to also use the `--create-namespace` option, as non-existent namespaces
do not get created by default (see [this](https://github.com/helm/helm/issues/6794)). 

##  Secure Environment

See Secure environment [documentation](SECURE-ENVIRONMENT.md)
