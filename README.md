# PHP Example

This is an example PHP application you can use to test your OSEv3 environment.

Here is an example:
```
user@host$ osc new-app openshift/php~https://github.com/christianh814/php-example-ose3
```

Things to keep in mind:
* `ose new-app` Creates a new application on OSE3
* `openshift/php` This tells OSEv3 to use the PHP image stream provided by OSE
* Provide the git URL for the project
  * Syntax is "imagestreat~souce"

NOTE: Current [bug](https://bugzilla.redhat.com/show_bug.cgi?id=1232003) has you manually update the `BuildConfig` to add the "openshift" namespace:

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
        **namespace: openshift**
```

Once you created the app, start your build
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
