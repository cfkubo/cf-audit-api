<p align="center">
<img src="Broadcom-vmware.png" width="300" alt="Online Boutique" />
</p>


### UAA TAS url:  https://uaa.SYS-DOMAIN

### UAA opsman url:  https://opman-url/uaa

### CF API url: https://api.SYS-DOMAIN/v3

### Login with admin client creds. The creds are under TAS tile credentials section for uaa "Admin Client Credentials"


# TAS UAA Target
```
uaac target uaa.sys.XXXXXXXXX.h2o.vmware.com  --skip-ssl-validation
```

```
Target: https://uaa.sys.XXXXXXXXX.h2o.vmware.com
Context: admin, from client admin
```

# Login
```
uaac token client get admin -s XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

```
WARNING: Decoding token without verifying it was signed by its authoring UAA

Successfully fetched token via client credentials grant.
Target: https://uaa.sys.XXXXXXXXX.h2o.vmware.com
Context: admin, from client admin
```

# Create UAA admin ready-only client

```
uaac client add arul6 -s arul6  --authorized_grant_types client_credentials --scope cloud_controller.admin_read_only,scim.read --authorities cloud_controller.admin_read_only,scim.read
```
### Sample output
```
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

# Create UAA Auditor ready-only client
```
uaac client add auditor1 -s auditor1   --authorized_grant_types client_credentials --scope cloud_controller.global_auditor --authorities cloud_controller.global_auditor
```
### Sample output
```
  scope: cloud_controller.global_auditor
  client_id: auditor1
  resource_ids: none
  authorized_grant_types: client_credentials
  autoapprove:
  authorities: cloud_controller.global_auditor
  name: auditor1
  required_user_groups:
  lastmodified: 1706119978000
  id: auditor1

```

# Fetch the bearer token via curl
### curl https://uaa.SYS-DOEMAIN/oauth/token

```
curl -k https://uaa.sys.XXXXXXXXX.h2o.vmware.com/oauth/token -u "auditor1:auditor1" -d grant_type=client_credentials
```

### Sample output
```
{"access_token":"eyJqdGkiOiIwNmFiZjZmZjFkNDg0NWFiOTUyYjQyZjAxZWYwNjg3OSIsInN1YiI6ImFydWw1IiwiYXV0aG9yaXRpZXMiOlsiY2xvdWRfY29udHJvbGxlci5hZG1pbl9yZWFkX29ubHkiXSwic2NvcGUiOlsiY2xvdWRfY29udHJvbGxlci5hZG1pbl9yZWFkX29ubHkiXSwiY2xpZW50X2lkIjoiYXJ1bDUiLCJjaWQiOiJhcnVsNSIsImF6cCI6ImFydWw1IiwiZ3JhbnRfdHlwZSI6ImNsaWVudF9jcmVkZW50aWFscyIsInJldl9zaWciOiJiZjAxY2Y5OSIsImlhdCI6MTcwNjA1MTgyOCwiZXhwIjoxNzA2MDk1MDI4LCJpc3MiOiJodHRwczovL3VhYS5zeXMuaDJvLTQtMTU0MDIuaDJvLnZtd2FyZS5jb20vb2F1dGgvdG9rZW4iLCJ6aWQiOiJ1YWEiLCJhdWQiOlsiYXJ1bDUiLCJjbG91ZF9jb250cm9sbGVyIl19.D17-H6d6oLqPMGW7dYNnKcngFYixosvmuJSEAhsNEJptjPDBB_Tsbi_r7xTGyojRXRs0RXdLqKHE9kyFl58vjIALbPqmGWJthxpTSHs0G-LhiR01ZLeoPAdSqjArY6HL6okpDRQOnJa2CtNvK75VUABH3LdFZ23H3saHBt0tZlz8_dNfIsDWl980xik-CPYd8ud4xWxITqDLO23J9EQO8ORd5xA-O7JhyU1aRgOk0nJSoG77OiqbBtD3AYRBD25w_x3WsnUne--W5wAxRtBHAJzEV8Gi16TtZqUzHlMGUVToq2u4-BzBylBPV7-1xqLLzZ9AJF434nxKt4ZQJ7RdVg","token_type":"bearer","expires_in":43199,"scope":"cloud_controller.admin_read_only","jti":"06abf6ff1d4845ab952b42f01ef06879"}%
```

# Fetch the apps endpoint via curl
### curl https://api.SYS-DOMIAN/v3/apps

```
curl https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891 -X GET -H "Authorization: bearer eyJqdGkiOiIwNmFiZjZmZjFkNDg0NWFiOTUyYjQyZjAxZWYwNjg3OSIsInN1YiI6ImFydWw1IiwiYXV0aG9yaXRpZXMiOlsiY2xvdWRfY29udHJvbGxlci5hZG1pbl9yZWFkX29ubHkiXSwic2NvcGUiOlsiY2xvdWRfY29udHJvbGxlci5hZG1pbl9yZWFkX29ubHkiXSwiY2xpZW50X2lkIjoiYXJ1bDUiLCJjaWQiOiJhcnVsNSIsImF6cCI6ImFydWw1IiwiZ3JhbnRfdHlwZSI6ImNsaWVudF9jcmVkZW50aWFscyIsInJldl9zaWciOiJiZjAxY2Y5OSIsImlhdCI6MTcwNjA1MTgyOCwiZXhwIjoxNzA2MDk1MDI4LCJpc3MiOiJodHRwczovL3VhYS5zeXMuaDJvLTQtMTU0MDIuaDJvLnZtd2FyZS5jb20vb2F1dGgvdG9rZW4iLCJ6aWQiOiJ1YWEiLCJhdWQiOlsiYXJ1bDUiLCJjbG91ZF9jb250cm9sbGVyIl19.D17-H6d6oLqPMGW7dYNnKcngFYixosvmuJSEAhsNEJptjPDBB_Tsbi_r7xTGyojRXRs0RXdLqKHE9kyFl58vjIALbPqmGWJthxpTSHs0G-LhiR01ZLeoPAdSqjArY6HL6okpDRQOnJa2CtNvK75VUABH3LdFZ23H3saHBt0tZlz8_dNfIsDWl980xik-CPYd8ud4xWxITqDLO23J9EQO8ORd5xA-O7JhyU1aRgOk0nJSoG77OiqbBtD3AYRBD25w_x3WsnUne--W5wAxRtBHAJzEV8Gi16TtZqUzHlMGUVToq2u4-BzBylBPV7-1xqLLzZ9AJF434nxKt4ZQJ7RdVg" -k
```
### Sample output
```
{"guid":"9d2d4811-9ecd-464b-8e80-901e17223891","created_at":"2024-01-23T02:25:47Z","updated_at":"2024-01-23T02:38:08Z","name":"rabbitmq-demo","state":"STARTED","lifecycle":{"type":"buildpack","data":{"buildpacks":[],"stack":"cflinuxfs4"}},"relationships":{"space":{"data":{"guid":"a0ff4e15-dd2c-45a7-9c65-615606ced810"}}},"metadata":{"labels":{},"annotations":{}},"links":{"self":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891"},"environment_variables":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/environment_variables"},"space":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/spaces/a0ff4e15-dd2c-45a7-9c65-615606ced810"},"processes":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/processes"},"packages":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/packages"},"current_droplet":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/droplets/current"},"droplets":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/droplets"},"tasks":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/tasks"},"start":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/actions/start","method":"POST"},"stop":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/actions/stop","method":"POST"},"revisions":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/revisions"},"deployed_revisions":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/revisions/deployed"},"features":{"href":"https://api.sys.XXXXXXXXX.h2o.vmware.com/v3/apps/9d2d4811-9ecd-464b-8e80-901e17223891/features"}}}%
```

# Fetch Users endpoint via curl
### curl https://api.SYS-DOMIAN/v3/users

```
curl -k 'https://uaa.sys.XXXXXXXXX.h2o.vmware.com/Users' -i -X GET \
    -H 'Accept: application/json' \
    -H 'Authorization: Bearer eyJqdGkiOiI5YmVhNDc4MjgyYjA0MGRlOThiYmU3OWNjZjNiNTY5OCIsInN1YiI6ImFydWw2IiwiYXV0aG9yabDYiLCJjaWQiOiJhcnVsNiIsImF6cCI6ImFydWw2IiwiZ3JhbnRfdHlwZSI6ImNsaWVudF9jcmVkZW50aWFscyIsInJldl9zaWciOiIxMzBlZGJlYyIsImlhdCI6MTcwNjA1MzgwMywiZXhwIjoxNzA2MDk3MDAzLCJpc3MiOi' \
    -H 'Content-Type: application/json' \
    -H 'If-Match: 0'
```

# Fetch Spaces endpoint via curl
### curl https://api.SYS-DOMIAN/v3/spaces

```
curl "https://api.sys.XXXXXXXXXX.h2o.vmware.com/v3/spaces" \
  -X GET \
  -H "Authorization: bearer NUSlR73FQlZcqOI1LrlkCWMnSmwy6og1zmtltbTC7tlVqa-W0CSJcvzqHE9igy5d7eN-84j08kTWRWi5opCCYdeEXhsGzWobiWDYXwhVyVKcYnxwCgofUVBPhYu4vsStnXfJ0NhPbA8qm54514eEZxnoElEM8KZKkQbgD-uYWuhjm9B_I-X4KkVMeANYXmTfs-J8W6RdAY9Dw_jgjfpchSdB2M-kdg8B4bDHz01qYNkxrLHq-sazkD1-" -k
```

# Fetch Orgs endpoint via curl
### curl https://api.SYS-DOMIAN/v3/organizations

```
curl "https://api.sys.XXXXXXXX.h2o.vmware.com/v3/organizations" \
  -X GET \
  -H "Authorization: bearer 3MiOiJodHRwczovL3VhYS5zeXMuaDJvLTQtMTU0MDIuaDJvLnZtd2FyZS5jb20vb2F1dGgvdG9rZW4iLCJ6aWQiOiJ1YWEiLCJhdWQiOlsiYXJ1bDUiLCJjbG91ZF9jb250cm9sbGVyIl19.I03cPZlkBxWCdXXgSPVbccRDAtCzzu86KBXbtS4mP0V8ct3_4JNQJHsNJ75eVTHLwkH9Y-PviGtIFFDxfSv71vKOoVXd2e4AY8jn4GwMWaR3oJY5QtWolCw" -k
```

# Fetch Roles endpoint via curl
### curl https://api.SYS-DOMIAN/v3/roles

```
curl -k "https://api.sys.h2o-4-15402.h2o.vmware.com/v3/roles" \
  -X GET \
  -H "Authorization: bearer eyMTI4LCJpc3MiOiJodHRwczovL3VhYS5zeXMuaDJvLTQtMTU0MDIuaDJvLnZtd2FyZS5jb20vb2F1dGgvdG9rZW4iLCJ6aWQiOiJ1YWEiLCJhdWQiOlsiY2xvdWRfY29udHJvbGxlciIsInNjaW0iLCJhcnVsNiJdfQ.PuU9HqVaKp2ot9uf7_qnAUavTGqOtEIoRH94U2MA-465ZI00wxiRwETwNF57540rEsmzu0XN-9GS4HUnmnK2cJBS0mE5H5t9PuEw65jMrjZiBTlK3OXipCHh6_rOR-JjCrjdH1QFS4UTxhuIgk8I463OK8Wk_Qk-ONRA31A1pY4t2ekvyeUO6RA00ydsvYf3xKl40VUQAHPIFKxsA9yg4-HYuDeSXgxGr3NUY5S-GfUj4KearG5pzr60fy55d0jVtx1NVacNUZj1cpcqF0uVrxU4RCK47-Hp7cHDhbe5kPCsBo4XKV9fxdkDp9478DoSLCTFJx41xIc0_OU_fm5sTg" \
  -H "Content-type: application/json"
```

# Fetch Spaces endpoint via curl

```
curl "https://api.example.org/v3/spaces" \
-X GET \
-H "Authorization: bearer [token]"
```

# Fetch Spaces Users endpoint via curl

```
curl "https://api.example.org/v3/spaces/:guid/users" \
  -X GET \
  -H "Authorization: bearer [token]"
```
