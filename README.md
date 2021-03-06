Dropwizard Elasticsearch
========================
[![Build Status](https://secure.travis-ci.org/joschi/dropwizard-elasticsearch.png?branch=master)](https://travis-ci.org/joschi/dropwizard-elasticsearch)

A set of classes for using [Elasticsearch] [1] in a [Dropwizard] [2] service.

The package provides a [lifecycle-managed] [3] client class (`ManagedEsClient`), a configuration class with the most
common options (`EsConfiguration`), and some [health checks] [4] which can instantly be used in any Dropwizard service.

[1]: http://www.elasticsearch.org/
[2]: http://dropwizard.codahale.com/
[3]: http://dropwizard.codahale.com/manual/core/#managed-objects
[4]: http://dropwizard.codahale.com/manual/core/#health-checks


Usage
-----

Just add `EsConfiguration` to your [Configuration](http://dropwizard.codahale.com/manual/core/#configuration) class and
create an `ManagedEsClient` instance in the run method of your service.

You can also add one of the existing health checks to your [Environment](http://dropwizard.codahale.com/manual/core/#environments)
in the same method. At least the usage of `EsClusterHealthCheck` is strongly advised.


    public class DemoService extends Service<DemoConfiguration> {
        // [...]
        @Override
        public void run(DemoConfiguration config, Environment environment) {
            ManagedEsClient managedClient = new ManagedEsClient(config.getEsConfiguration());
            environment.manage(managedClient);
            environment.addHealthCheck(new EsClusterHealthCheck(managedClient.getClient()));
            // [...]
        }
    }


Configuration
-------------

The following configuration settings are supported by `EsConfiguration`:

* `nodeClient`: When `true`, `ManagedEsClient` will create a `NodeClient`, otherwise a `TransportClient`; default: `true`
* `servers`: A list of servers for usage with the created TransportClient if `nodeClient` is `false`
* `clusterName`: The name of the Elasticsearch cluster; default: "elasticsearch"
* `settings`: Any additional settings for Elasticsearch, see [Configuration](http://www.elasticsearch.org/guide/reference/setup/configuration/)

An example configuration file for creating a Node Client could like this:

    clusterName: MyClusterName
    settings:
      node.name: MyCustomNodeName


Drawbacks
---------

None known. :)


Maven Artifacts
---------------

This project is available on Maven Central. To add it to your project simply add the following dependencies to your
`pom.xml`:

    <dependency>
      <groupId>com.github.joschi</groupId>
      <artifactId>dropwizard-elasticsearch</artifactId>
      <version>0.1.0</version>
    </dependency>


Support
-------

Please file bug reports and feature requests in [GitHub issues](https://github.com/joschi/dropwizard-elasticsearch/issues).


Acknowledgements
----------------

Thanks to Alexander Reelsen (@spinscale) for his [Dropwizard Blog Sample](https://github.com/spinscale/dropwizard-blog-sample)
which sparked the idea for this project.


License
-------

Copyright (c) 2013 Jochen Schalanda

This library is licensed under the Apache License, Version 2.0.

See http://www.apache.org/licenses/LICENSE-2.0.html or the LICENSE file in this repository for the full license text.