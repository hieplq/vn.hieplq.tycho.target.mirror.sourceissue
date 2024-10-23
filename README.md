## Test to include the source artifact in the mirrored target repository
### on my target platforn
1. p2 create by org.eclipse.equinox.p2.publisher.FeaturesAndBundlesPublisher: https://idempiere.github.io/binary.file/p2.repackaged/12.0.0
2. p2 normal: https://download.eclipse.org/eclipse/updates/4.29
3. maven artifact source artifact already has OSGi Bundle Manifest Headers

### Run test
```
git clone git@github.com:hieplq/vn.hieplq.tycho.target.mirror.sourceissue.git
cd vn.hieplq.tycho.target.mirror.sourceissue/com.codeandme.tycho.releng
mvn verify
ls -1 ../com.codeandme.tycho.releng.targetplatform/target/target-platform-repository/plugins
```

### List artifact output on mirror target
```
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

### Conclude
1. artifact on p2 repository created using org.eclipse.equinox.p2.publisher.FeaturesAndBundlesPublisher are automatically included source in the mirror
2. artifact on every p2 can explicitly include source by use ```unit id="org.opentest4j.source" version="1.3.0"/>```
3. artifact that source has OSGi Bundle Manifest Headers on maven can explicitly include by use ```<classifier>sources</classifier>```
4. source artifact without OSGi Bundle Manifest Headers on maven can't add to mirror

## workaround
download/mirror p2 repository and recreate it by run
   ```
   eclipse -application org.eclipse.equinox.p2.publisher.FeaturesAndBundlesPublisher \
    -metadataRepository file:$PWD \
    -artifactRepository file:$PWD \
    -source $PWD \
    -publishArtifact
   ```

**NOTE**

goal [mirror-target-platform](https://tycho.eclipseprojects.io/doc/latest/target-platform-configuration/mirror-target-platform-mojo.html) from tycho target-platform-configuration help create p2 repository at local but without source bundle so hard to use it for debug

on eclipse target always include source for easy debug but tycho for terminal it don't need source

the feature [targetDefinitionIncludeSource](https://tycho.eclipseprojects.io/doc/latest/target-platform-configuration/target-platform-configuration-mojo.html#targetDefinitionIncludeSource) is added for [case need source on terminal](https://bugs.eclipse.org/bugs/show_bug.cgi?id=411664#c20) 

but seem targetDefinitionIncludeSource isn't support on some case

1. p2 isn't create by org.eclipse.equinox.p2.publisher.FeaturesAndBundlesPublisher
2. maven repository
