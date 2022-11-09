Fix for Xpect Update-Site
=========================

[Xpect](https://www.xpect-tests.org/) is a framework for testing DSL based on the Xtext framework.
It offers an [Eclipse Update Site][xpect-update] to install the required Eclipse plugins.
But currently (2022-11-09) this update site is broken. This repository is a partial fix for that.

The problem: Xpect requires additional plugins (JUnit extensions) which live in a separate update site.
The Xpect site is a "composite site" which includes

1)	a link to the JUnit extension,
2)	the Xpect plugins and features in a subdirectory.

The composite site is defined by to files: `compositeContent.xml` and `compositeArtifacts.xml`.

**compositeContent.xml:**

```
<?xml version='1.0' encoding='UTF-8'?>
<?compositeMetadataRepository version='1.0.0'?>
<repository name='&quot;Xpect Update Site&quot;'
    type='org.eclipse.equinox.internal.p2.metadata.repository.CompositeMetadataRepository' version='1.0.0'>
  <properties size='1'>
    <property name='p2.timestamp' value='1243822502499'/>
  </properties>
 <children size='2'>
    <child location='http://meysholdt.github.com/eclipse_jdt_junit_runners/updatesite/'/>
    <child location='local'/>
  </children>
</repository>
```

**compositeArtifacts.xml:**

```
<?xml version='1.0' encoding='UTF-8'?>
<?compositeArtifactRepository version='1.0.0'?>
<repository name='&quot;Xpect Update Site&quot;'
    type='org.eclipse.equinox.internal.p2.artifact.repository.CompositeArtifactRepository' version='1.0.0'>
  <properties size='1'>
    <property name='p2.timestamp' value='1243822502440'/>
  </properties>
  <children size='2'>
    <child location='http://meysholdt.github.com/eclipse_jdt_junit_runners/updatesite/'/>
    <child location='local'/>
  </children>
</repository>
```

In both cases the link to `eclipse_jdt_junit_runners` is broken. The Github [Issue #6](https://github.com/meysholdt/eclipse_jdt_junit_runners/issues/6)
(2021-05-20) is not new and hence will probably not be solved soon.

But the data for the JUnit Runner update site is available in the `gh-pages` branch of the project in the directory `/updatesite/`.
The data can be fetched with URLs like this:

https://raw.githubusercontent.com/meysholdt/eclipse_jdt_junit_runners/gh-pages/updatesite/site.xml

So this repository defines a new, top-level update site for Xpect in `/meta-site`. This update site can be addressed
with this URL.

	https://raw.githubusercontent.com/asheiduk/xpect-mirror-fixed/master/meta-site/

Additionally a simple Maven-POM is provided, which mirrors the new site and – recursively – the contained
update sites into `target/repository`. This materialized site can be zipped and then used in environments behind
cooperate firewalls.

[xpect-update]: http://www.xpect-tests.org/updatesite/nightly/
