# PHP Example

This is an example PHP application you can use to test your OSEv3 environment.

Here is an example:
```
user@host$ osc new-app --build=openshift/php https://github.com/christianh814/php-example-ose3
```

Things to keep in mind:
* `ose new-app` Creates a new application on OSE3
* `--build=openshift/php` This tells OSEv3 to use the PHP builder provided by OSE
* Provide the git URL for the project

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
