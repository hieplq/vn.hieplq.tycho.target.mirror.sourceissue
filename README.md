## Test mirror target to show which artifact output with source
```
git clone git@github.com:hieplq/vn.hieplq.tycho.target.mirror.sourceissue.git
cd vn.hieplq.tycho.target.mirror.sourceissue/com.codeandme.tycho.releng
mvn verify
ls -1 ../com.codeandme.tycho.releng.targetplatform/target/target-platform-repository/plugins
```

```
# list of artifact on plugins
org.apache.commons.lang_2.6.0.jar
org.atmosphere.runtime_2.7.2.v202109010034.jar
org.atmosphere.runtime.source_2.7.2.v202109010034.jar
org.eclipse.jetty.server_12.0.14.jar
org.eclipse.jetty.server.source_12.0.14.jar
org.eclipse.text_3.13.100.v20230801-1334.jar
org.opentest4j_1.3.0.jar
org.opentest4j.source_1.3.0.jar
wrapped.org.apache.lucene.lucene-core_8.4.1.jar
```

goal [mirror-target-platform](https://tycho.eclipseprojects.io/doc/latest/target-platform-configuration/mirror-target-platform-mojo.html) from tycho target-platform-configuration help create p2 repository at local but without source bundle so hard to use it for debug

on eclipse target always include source for easy debug but tycho for terminal it don't need source

the feature [targetDefinitionIncludeSource](https://tycho.eclipseprojects.io/doc/latest/target-platform-configuration/target-platform-configuration-mojo.html#targetDefinitionIncludeSource) is added for [case need source on terminal](https://bugs.eclipse.org/bugs/show_bug.cgi?id=411664#c20) 

but seem targetDefinitionIncludeSource isn't support on some case

1. p2 isn't create by org.eclipse.equinox.p2.publisher.FeaturesAndBundlesPublisher
2. maven repository

## workaround
1. explicitly add source artifact
   * p2 repository
     
     ```unit id="org.opentest4j.source" version="1.3.0"/>```
   * maven repository with source already has OSGi Bundle Manifest Headers
     
     add ```<classifier>sources</classifier>```
3. download/mirror p2 repository and recreate it by run
   ```
   eclipse -application org.eclipse.equinox.p2.publisher.FeaturesAndBundlesPublisher \
    -metadataRepository file:$PWD \
    -artifactRepository file:$PWD \
    -source $PWD \
    -publishArtifact
   ```
## case still not found workaround
1. on maven source artifact hasn't OSGi Bundle Manifest Headers
