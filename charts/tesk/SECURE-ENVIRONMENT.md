# Secure environment deployment

New values have been added to deploy into a secure environment (no internet access such as cPouta)

- `.Values.tesk.proxy`: Deploy a `ConfigMap`
- `.Values.tesk.javaOpt`: Add `JAVA_OPTS` environment variable for tesk-api
- `.Values.oauth2`: Deploy a new container in tesk-api pod

`proxy` and `javaOpt` are used for the tesk-api pod and containers inside the pod to access internet through a proxy (Squid proxy)

## Oauth2-proxy

You can deploy an `oauth2-proxy` in front of tesk-api to force LSAAI login. Once the user is logged in, he will be redirected to the Swagger UI

The user can also access `/token` to retrieve the  LSAAI token. This token can be used to submit tasks to the API or consult the status of the tasks

```
curl -H "Authorization: Bearer <token>" https://your.teskapi.com/ga4gh/tes/v1/tasks
```

To be able to use LSAAI, you must submit a new service or update a current one at this address: https://services.aai.lifescience-ri.eu/spreg/