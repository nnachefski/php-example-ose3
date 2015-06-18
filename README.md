# PHP Example

**BEFORE YOU PERFORM THESE STEPS READ THE "NOTE" SECTION BELOW**

This is an example PHP application you can use to test your OSEv3 environment.

Here is an example:
```
user@host$ osc new-app openshift/php~https://github.com/christianh814/php-example-ose3
```

Things to keep in mind:
* `ose new-app` Creates a new application on OSE3
* `openshift/php` This tells OSEv3 to use the PHP image stream provided by OSE
* Provide the git URL for the project
  * Syntax is "imagestream~souce"

Once you created the app, start your build (before you start your build make sure you read [this](https://github.com/christianh814/php-example-ose3/blob/master/README.md#command-line-building))

```
user@host$ osc start-build php-example-ose3
```

Once the build completes; create and add your route:
```
user@host$ cat php-route.json
{
  "kind": "Route",
  "apiVersion": "v1beta3",
  "metadata": {
    "name": "php-example-route"
  },
  "spec": {
    "host": "php-example.cloudapps.example.com",
    "to": {
      "name": "php-example-ose3"
    }
  }
}
user@host$ osc php-route.json
```

OSEv3 Is still in beta. Please consult the [documentation](https://github.com/openshift/training)

## NOTE:

### Project Namespace
Make sure the namespace you're working on has [quotas](https://github.com/openshift/training/blob/master/beta-4-setup.md#applying-quota-to-projects) and a [limit](https://github.com/openshift/training/blob/master/beta-4-setup.md#applying-limit-ranges-to-projects) set

### Command-line Building
Current [bug](https://bugzilla.redhat.com/show_bug.cgi?id=1232003) has you manually update the `BuildConfig` to add the "namespace: openshift" if you're building from the `new-app` command

```
user@host$ ose edit bc/php-example-ose3
```

File should have something like this
```
  strategy:
    sourceStrategy:
      from:
        kind: ImageStreamTag
        name: php:latest
        namespace: openshift
```

Once you do this you can start the build. 

### Scaling

If you want to scale this app you need to perform these steps [explained here](https://github.com/openshift/training/blob/master/beta-4-setup.md#implications-of-quota-enforcement-on-scaling) because of a bug.

Before you scale the application, you need to update the deployment config to put a memory and CPU limit on the pods (use json so you don't have to worry about the yaml indenting):

```
user@host$ osc edit dc/php-example-ose3 -o json
```

Find the `spec` section that has the `containers` block. There find the `resources` block. Update that block to look like this:

```
    "resources": {
      "limits": {
        "cpu": "10m",
        "memory": "16Mi"
      }
    },
```

Now you can scale the application

```
user@host$ osc resize --replicas=3 rc/php-example-ose3-1
```

**NOTE:** You need to specify the build number (aka the `-1` part) to scale the right build
