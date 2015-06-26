# PHP Example

**BEFORE YOU PERFORM THESE STEPS READ THE "NOTE" SECTION BELOW**

This is an example PHP application you can use to test your OSEv3 environment.

Here is an example:
```
user@host$ oc new-app openshift/php~https://github.com/christianh814/php-example-ose3
```

Things to keep in mind:
* `ose new-app` Creates a new application on OSE3
* `openshift/php` This tells OSEv3 to use the PHP image stream provided by OSE
* Provide the git URL for the project
  * Syntax is "imagestream~souce"

Once you created the app, start your build

```
user@host$ oc start-build php-example-ose3
```

Once the build completes; create and add your route:
```
user@host$ oc expose service php-example-ose3 --hostname=php-example.cloudapps.example.com
```

Scale up as you wish
```
user@host$ oc scale --replicas=3 dc/php-example-ose3
```

If you'd like to add another route (aka "alias"); then you need to specify a new name for it

```
user@host$ oc expose service php-example-ose3 --name=hello-openshift --hostname=hello-openshift.cloudapps.example.com
``

## NOTE:

### Project Namespace
Make sure the namespace you're working on has [quotas](https://github.com/openshift/training/blob/master/beta-4-setup.md#applying-quota-to-projects) and a [limit](https://github.com/openshift/training/blob/master/beta-4-setup.md#applying-limit-ranges-to-projects) BOTH set (you can have none or both...but not one or the other)
