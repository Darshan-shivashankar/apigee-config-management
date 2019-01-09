# apigee-config-maven-plugin

Maven plugin to create, manage Apigee config like Cache, KVM, Target Server, Resource Files, API Products.

Help API teams follow API development best practices with Apigee.


 - Track Apigee Config (KVM, cache, target servers, etc.) in source
   control



 - Deploy config changes along with the API in a CI pipeline



 - Simplify, automate config management during API development



 - Track config changes in prod environment as releases

Read this document further for plugin usage instructions.

## Plugin Usage

    mvn install -Ptest -Dapigee.config.options=create

      # Options

      -P<profile>
        Pick a profile in the parent pom.xml (shared-pom.xml in the example).
        Apigee org and env information comes from the profile.

      -Dapigee.config.options
        none   - No action (default)
        create - Create when not found. Pre-existing config is NOT updated even if it is different.
        update - Update when found; create when not found, updates individual entries for kvms. Refreshes all config to reflect edge.json.
        delete - Delete all config listed in edge.json.
        sync   - Delete and recreate.

      -Dapigee.config.file=<path-to-config>
         path containing the configuration.

      -Dapigee.config.dir=<dir>
         directory containing multi-file format config files.

      -Dapigee.config.exportDir=<dir>
         dir where the dev app keys are exported. This is only used for `exportAppKeys` goal. The file name is always devAppKeys.json

      # Individual goals
      You can also work with an individual config type using the
      corresponding goal directly. The goals available are,

      caches
      kvms
      targetservers
      resourcefiles
      flowhooks
      maskconfigs
      apiproducts
      developers
      apps
      virtualhosts
      exportAppKeys


      For example, the apps goal is used below to only create apps and ignore all other config types.
      mvn apigee-config:apps -Ptest -Dapigee.config.options=create

      To export the dev app keys, use the following:
      mvn apigee-config:exportAppKeys -Ptest -Dapigee.config.exportDir=./target




To use, clone this repo and edit oapi-apigee-config/shared-pom.xml, and update org and env elements in all profiles to point to your Apigee org, env. You can add more profiles corresponding to each env in your org.

      <apigee.org>myorg</apigee.org>
      <apigee.env>test</apigee.env>


To run the plugin and use the multi-file config format jump to samples project `cd /oapi-apigee-config` and run

`mvn -X install -P<<profileid>> -Dusername=<<username>> -Dpassword=<<password>> -Dapigee.config.options=update`

## Multi-file config
Projects with several config entities can utilize the multi-file structure to organize config while keeping individual file sizes within manageable limits. The plugin requires the use of specific file names and directories to organize config.

The apigee.config.dir option must be used to identify the top most directory containing the following config structure.


      ├── api
      │   ├── forecastweatherapi
      │   │   ├── resourceFiles
      │   │   │   ├── jsc
      │   │   │   │    ├── test.js
      │   │   ├── kvms.json
      │   │   └── resourcefiles.json
      │   └── oauth
      │       ├── kvms.json
      │       └── maskconfigs.json
      ├── env
      │   ├── prod
      │   │   ├── caches.json
      │   │   └── flowhooks.json
      │   └── test
      │       ├── caches.json
      │       ├── kvms.json
      │       ├── targetServers.json
      │       └── virtualHosts.json   
      └── org
          ├── apiProducts.json
          ├── developerApps.json
          ├── developers.json
          ├── kvms.json
          └── maskconfigs.json
