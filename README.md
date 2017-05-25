# imagepromotion
Artifacts and walk through to illustrate image promotion.

To try this download the latest oc tool [download](https://github.com/openshift/origin/releases) and install it to a known location

```
sudo mv ~/Downloads/ocdownloadfolder/oc /usr/local/bin
sudo chmod +x /usr/local/bin/oc
```
Set up a local OpenShift cluster

```
oc cluster up
```


Next login and create two new projects:

```
oc login
oc new-project env1
oc new-project env2
```

In the env1 project click "add to project" click "Javascript" and then the "node.js" sample app. 

**Important:** name the app "node-app"

Apply the correct role to allow ```env2``` to pull images from ```env1```
```
oc policy add-role-to-group system:image-puller system:serviceaccounts:env2 -n env1
```

Now move to env2

```
oc project env2
```

Setup the required Objects using the env2.json template

```
oc new-app -f env2.json

```

Back in the env1 project apply the correct tag to that latest build of the node-app imagestream

```
oc project env1
oc tag env1/node-app:latest env2/node-app:test

```

You should see the deployment in env2 update now and run the image env2/node-app:test.

If you go back to env1 and kick off another build and retag env2 will automatically be updated to the new image.



