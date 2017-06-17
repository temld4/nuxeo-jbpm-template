## Nuxeo JBPM template

### Description

This is a package for deploying jBPM workflows to Nuxeo. You can use this package as a template and with a few simple steps you are able to add your own workflow to the package and deploy it.

### Prerequisities

Let us use the following designations:

`$NUXEO_SERVER_PATH` - full path to Nuxeo Server root folder

`$PACKAGE_PATH` - full path to a folder where JBPM template package will be after build process, in our case it is `nuxeo-jbpm-template-package/target`

`nuxeo-jbpm-template-package-1.0-SNAPSHOT.zip` - name of the Nuxeo JBPM template package file

NOTE: You also need `nuxeo-jbpm-package` to be installed, see https://github.com/temld4/nuxeo-jbpm-bin/blob/master/README.md for installation instructions

### Instructions

Here are the steps following which you will be able to deploy jBPM workflow to Nuxeo:

Suppose your process-definition file is `my-workflow.xml` and the workflow has a name of `my-wf-name`.

1. Put your process-definition file into `nuxeo-jbpm-default/src/main/resources/process` folder.

2. Add corresponding lines to `nuxeo-jbpm-default/src/main/resources/OSGI-INF/default-process-definitions-contrib.xml`:


<?xml version="1.0"?>
<component name="org.nuxeo.ecm.platform.jbpm.core.defaultWfContrib">

    <extension target="org.nuxeo.ecm.platform.jbpm.core.JbpmService"
               point="processDefinition">
               
        <!-- Other content -->
        <processDefinition path="process/my-workflow.xml" deployer="nuxeoProperties" />
    </extension>

    <extension target="org.nuxeo.ecm.platform.jbpm.core.JbpmService"
               point="securityPolicy">
               
        <!-- Other content -->
        <securityPolicy
                class="org.nuxeo.ecm.platform.jbpm.core.service.DefaultJbpmSecurityPolicy"
                for="my-wf-name" />
    </extension>

    <extension target="org.nuxeo.ecm.platform.jbpm.core.JbpmService"
               point="typeFilter">

        <type name="Note">
            <!-- Other content -->
            <processDefinition>my-wf-name</processDefinition>
        </type>
        <type name="File">
            <!-- Other content -->
            <processDefinition>my-wf-name</processDefinition>
        </type>
    </extension>

</component>

3. Build the package - from the package's root folder execute
    
    `mvn clean package -DskipTests`
    
4. Install the package

    `$NUXEO_SERVER_PATH/bin/nuxeoctl mp-install $PACKAGE_PATH/nuxeo-jbpm-template-package-1.0-SNAPSHOT.zip`


