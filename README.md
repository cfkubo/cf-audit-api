<p align="center">
<img src="Broadcom-vmware.png" width="300" alt="Online Boutique" />
</p>


### UAA TAS url:  uaa.<sys.domain> for tas

### UAA opsman url:  <<opmanurl>>/uaa

### Target uaac client to TAS uaa

### Login with admin client creds. The creds are under TAS tile credentials section for uaa "client admin credentials"


# TAS UAA Target
```
 tas % uaac target uaa.sys.XXXXXXXXX.h2o.vmware.com  --skip-ssl-validation
uaac target : uaac target _

Target: https://uaa.sys.XXXXXXXXX.h2o.vmware.com
Context: admin, from client admin
```

# Login
```
 tas % uaac token client get admin -s XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

WARNING: Decoding token without verifying it was signed by its authoring UAA

Successfully fetched token via client credentials grant.
Target: https://uaa.sys.XXXXXXXXX.h2o.vmware.com
Context: admin, from client admin
```

# Create UAA admin readyonly client

```
 tas % uaac client add arul6 -s arul6  --authorized_grant_types client_credentials --scope cloud_controller.admin_read_only,scim.read --authorities cloud_controller.admin_read_only,scim.read

  scope: cloud_controller.admin_read_only scim.read
  client_id: arul6
  resource_ids: none
  authorized_grant_types: client_credentials
  autoapprove:
  authorities: cloud_controller.admin_read_only scim.read
  name: arul6
  required_user_groups:
  lastmodified: 1706051966000
  id: arul6

```
# Fetch the brearer token via curl

```
 tas % curl -k https://uaa.sys.XXXXXXXXX.h2o.vmware.com/oauth/token -u "arul5:arul5" -d grant_type=client_credentials

{"access_token":"eyJqdGkiOiIwNmFiZjZmZjFkNDg0NWFiOTUyYjQyZjAxZWYwNjg3OSIsInN1YiI6ImFydWw1IiwiYXV0aG9yaXRpZXMiOlsiY2xvdWRfY29udHJvbGxlci5hZG1pbl9yZWFkX29ubHkiXSwic2NvcGUiOlsiY2xvdWRfY29udHJvbGxlci5hZG1pbl9yZWFkX29ubHkiXSwiY2xpZW50X2lkIjoiYXJ1bDUiLCJjaWQiOiJhcnVsNSIsImF6cCI6ImFydWw1IiwiZ3JhbnRfdHlwZSI6ImNsaWVudF9jcmVkZW50aWFscyIsInJldl9zaWciOiJiZjAxY2Y5OSIsImlhdCI6MTcwNjA1MTgyOCwiZXhwIjoxNzA2MDk1MDI4LCJpc3MiOiJodHRwczovL3VhYS5zeXMuaDJvLTQtMTU0MDIuaDJvLnZtd2FyZS5jb20vb2F1dGgvdG9rZW4iLCJ6aWQiOiJ1YWEiLCJhdWQiOlsiYXJ1bDUiLCJjbG91ZF9jb250cm9sbGVyIl19.D17-H6d6oLqPMGW7dYNnKcngFYixosvmuJSEAhsNEJptjPDBB_Tsbi_r7xTGyojRXRs0RXdLqKHE9kyFl58vjIALbPqmGWJthxpTSHs0G-LhiR01ZLeoPAdSqjArY6HL6okpDRQOnJa2CtNvK75VUABH3LdFZ23H3saHBt0tZlz8_dNfIsDWl980xik-CPYd8ud4xWxITqDLO23J9EQO8ORd5xA-O7JhyU1aRgOk0nJSoG77OiqbBtD3AYRBD25w_x3WsnUne--W5wAxRtBHAJzEV8Gi16TtZqUzHlMGUVToq2u4-BzBylBPV7-1xqLLzZ9AJF434nxKt4ZQJ7RdVg","token_type":"bearer","expires_in":43199,"scope":"cloud_controller.admin_read_only","jti":"06abf6ff1d4845ab952b42f01ef06879"}%
```

# Fetch the apps endpoint via curl

```
 tas % curl https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891 -X GET -H "Authorization: bearer eyJqdGkiOiIwNmFiZjZmZjFkNDg0NWFiOTUyYjQyZjAxZWYwNjg3OSIsInN1YiI6ImFydWw1IiwiYXV0aG9yaXRpZXMiOlsiY2xvdWRfY29udHJvbGxlci5hZG1pbl9yZWFkX29ubHkiXSwic2NvcGUiOlsiY2xvdWRfY29udHJvbGxlci5hZG1pbl9yZWFkX29ubHkiXSwiY2xpZW50X2lkIjoiYXJ1bDUiLCJjaWQiOiJhcnVsNSIsImF6cCI6ImFydWw1IiwiZ3JhbnRfdHlwZSI6ImNsaWVudF9jcmVkZW50aWFscyIsInJldl9zaWciOiJiZjAxY2Y5OSIsImlhdCI6MTcwNjA1MTgyOCwiZXhwIjoxNzA2MDk1MDI4LCJpc3MiOiJodHRwczovL3VhYS5zeXMuaDJvLTQtMTU0MDIuaDJvLnZtd2FyZS5jb20vb2F1dGgvdG9rZW4iLCJ6aWQiOiJ1YWEiLCJhdWQiOlsiYXJ1bDUiLCJjbG91ZF9jb250cm9sbGVyIl19.D17-H6d6oLqPMGW7dYNnKcngFYixosvmuJSEAhsNEJptjPDBB_Tsbi_r7xTGyojRXRs0RXdLqKHE9kyFl58vjIALbPqmGWJthxpTSHs0G-LhiR01ZLeoPAdSqjArY6HL6okpDRQOnJa2CtNvK75VUABH3LdFZ23H3saHBt0tZlz8_dNfIsDWl980xik-CPYd8ud4xWxITqDLO23J9EQO8ORd5xA-O7JhyU1aRgOk0nJSoG77OiqbBtD3AYRBD25w_x3WsnUne--W5wAxRtBHAJzEV8Gi16TtZqUzHlMGUVToq2u4-BzBylBPV7-1xqLLzZ9AJF434nxKt4ZQJ7RdVg" -k

{"guid":"9d2d4811-9ecd-464b-8e80-901e17223891","created_at":"2024-01-23T02:25:47Z","updated_at":"2024-01-23T02:38:08Z","name":"rabbitmq-demo","state":"STARTED","lifecycle":{"type":"buildpack","data":{"buildpacks":[],"stack":"cflinuxfs4"}},"relationships":{"space":{"data":{"guid":"a0ff4e15-dd2c-45a7-9c65-615606ced810"}}},"metadata":{"labels":{},"annotations":{}},"links":{"self":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891"},"environment_variables":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/environment_variables"},"space":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/spaces/a0ff4e15-dd2c-45a7-9c65-615606ced810"},"processes":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/processes"},"packages":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/packages"},"current_droplet":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/droplets/current"},"droplets":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/droplets"},"tasks":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/tasks"},"start":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/actions/start","method":"POST"},"stop":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/actions/stop","method":"POST"},"revisions":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/revisions"},"deployed_revisions":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/revisions/deployed"},"features":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/features"}}}%
```

# Fetch the users endpoint via curl

```
curl -k 'https://uaa.sys.XXXXXXXXX.h2o.vmware.com/Users' -i -X GET \
    -H 'Accept: application/json' \
    -H 'Authorization: Bearer eyJqdGkiOiI5YmVhNDc4MjgyYjA0MGRlOThiYmU3OWNjZjNiNTY5OCIsInN1YiI6ImFydWw2IiwiYXV0aG9yaXRpZXMiOlsiY2xvdWRfY29udHJvbGxlci5hZG1pbl9yZWFkX29ubHkiLCJzY2ltLnJlYWQiXSwic2NvcGUiOlsiY2xvdWRfY29udHJvbGxlci5hZG1pbl9yZWFkX29ubHkiLCJzY2ltLnJlYWQiXSwiY2xpZW50X2lkIjoiYXJ1bDYiLCJjaWQiOiJhcnVsNiIsImF6cCI6ImFydWw2IiwiZ3JhbnRfdHlwZSI6ImNsaWVudF9jcmVkZW50aWFscyIsInJldl9zaWciOiIxMzBlZGJlYyIsImlhdCI6MTcwNjA1MzgwMywiZXhwIjoxNzA2MDk3MDAzLCJpc3MiOiJodHRwczovL3VhYS5zeXMuaDJvLTQtMTU0MDIuaDJvLnZtd2FyZS5jb20vb2F1dGgvdG9rZW4iLCJ6aWQiOiJ1YWEiLCJhdWQiOlsiY2xvdWRfY29udHJvbGxlciIsInNjaW0iLCJhcnVsNiJdfQ.io8yNu-gNlMsaBxlnG4ZB-QOVMa2g98UVded1mQnx9Ba3cWGkEo-_h-qbSLgoVjLO4VU8BgMWYTh_NGa2P1qpxF1O1ySZXDTToAulUsJQoxSjoFnfWAbtpehCtgA0uuXYpEpbPGjaQm38jOWPFu8CsC0CEAFDZ9-blrDNbWVaWsZTULGFqevBFo6_qizYaxXf4y3VRsyajLu7caqoz2a9DLU7hDcrbciiMJ9L8Vx0c6EecAMaCUA8utQp73qBAkzZeOaRQ3K0cGl9rmjm_u0pdvCKksrLWF1eGSw31ACwsoMFHrFyjUH0jtN1M_puhdKTxcCJ8fJSlCdr8jIghwiIA' \
    -H 'Content-Type: application/json' \
    -H 'If-Match: 0'
```
