# Changes

## Version 11

* 2014-05-09 - Require Maven 3.1.1
* 2014-05-09 - Upgrade release plugin to 2.5.0 (from 2.4.2)

## Version 10

* 2014-02-20 - Upgrade guava to 16.0.1 (from 15.0)

## Version 9

* 2013-11-27 - Remove dep.findbugs-annotations.version, use dep.findbugs.version instead.
* 2013-11-25 - Remove all PMD rulesets except braces, PMD turns out to
               be more and more annyoing; rules need to be actively
               curated.
               See https://github.com/kitei/kitei-rules/blob/master/src/main/resources/pmd/kitei-standard.xml
               for an example
* 2013-11-25 - Upgrade commons-configuration dependency to 1.10 (from 1.9)
* 2013-11-25 - Upgrade maven-duplicate-finder-plugin plugin to 1.0.5 (from 1.0.4)
* 2013-11-25 - Upgrade findbugs plugin to 2.5.3 (from 2.5.2)
* 2013-11-25 - Upgrade deploy plugin to 2.8.1 (from 2.7)
* 2013-11-25 - Upgrade install plugin to 2.5.1 (from 2.5)
* 2013-11-25 - Upgrade release plugin to 2.4.2 (from 2.4.1)
* 2013-11-25 - Upgrade license plugin to 2.5 (from 2.2)
* 2013-11-25 - Enforce findbugs 2.0.2 in the findbugs plugin
* 2013-10-31 - Fix surefire argline to work with code coverage
* 2013-10-28 - Enforce PMD 5.0.5 in the PMD plugin
* 2013-10-28 - Only fail PMD on level 4 and higher

## Version 8

* 2013-10-23 - Revert javax.servlet dependency back to 3.0.1 (from 3.1.0)
* 2013-10-23 - Upgrade findbugs-annotations dependency to 2.0.2 (from 2.0.1)
* 2013-10-23 - Upgrade testng dependency to 6.8.7 (from 6.8.5)
* 2013-10-23 - Upgrade objenesis dependency to 2.1 (easymock needs 2.x) (from 1.3)
* 2013-10-23 - Upgrade jodatime to 2.3 (from 2.2)
* 2013-10-23 - Upgrade surefire plugin to 2.16 (from 2.15)
* 2013-10-23 - Upgrade install plugin to 2.5 (from 2.4)
* 2013-10-16 - Upgrade guava to 15.0


## Version 7

* 2013-08-14 - Also exclude the various org.mortbay.jetty:servlet* deps
* 2013-08-14 - Add mockito dependency (1.9.5)
* 2013-08-14 - Upgrade enforcer plugin to 1.3.1 (from 1.3)
* 2013-08-14 - Change from com.mycila.maven-license-plugin:maven-license-plugin:1.10.b1
               to com.mycila:license-maven-plugin:2.2
* 2013-08-14 - add *.proto and *.vm to list of excluded files for licenses
* 2013-08-14 - Upgrade commons-codec dependency to 1.8 (from 1.7)
* 2013-08-14 - Upgrade logback dependency to 1.0.13 (from 1.0.9)
* 2013-08-14 - Upgrade javax.servlet dependency to 3.1.0 (from 3.0.1)
* 2013-08-14 - Upgrade joda-time dependency to 2.2 (from 2.1)
* 2013-08-14 - Upgrade easymock dependency to 3.2 (from 3.1)
* 2013-08-14 - Upgrade slf4j dependency to 1.7.2 (from 1.7.5)
* 2013-08-14 - Upgrade testng dependency to 6.8.5 (from 6.8)


## Version 6

* 2013-07-16 - move file encoding in surefire onto the argline.
* 2013-07-15 - re-enable junit:junit with versions >= 4.11 (they fixed the deps)
* 2013-07-02 - remove mavanagaiata plugin, not really usable in its current state.
* 2013-07-12 - Require maven 3.0.5 to build
* 2013-07-12 - Upgrade compiler plugin to 3.1 (from 3.0)
* 2013-07-12 - Upgrade dependency plugin to 2.8 (from 2.7)
* 2013-07-12 - Upgrade enforcer plugin to 1.3 (from 1.2)
* 2013-07-12 - Upgrade javadoc plugin to 2.9.1 (from 2.9)
* 2013-07-12 - Upgrade project-info-reports plugin to 2.7 (from 2.6)
* 2013-07-12 - Upgrade release plugin to 2.4.1 (from 2.3.2)
* 2013-07-12 - Upgrade site plugin to 3.3 (from 3.2)
* 2013-07-12 - Upgrade surefire plugin to 2.15 (from 2.14)
* 2013-07-12 - Upgrade build-helper plugin to 1.8 (from 1.7)
* 2013-07-12 - Upgrade jacoco plugin to 0.6.3 (from 0.6.2)


## Version 5

* 2013-03-13 - Upgrade surefire plugin to 2.14 (from 2.13)
* 2013-03-13 - Upgrade pmd plugin to 3.0.1 (from 2.7.1)
* 2013-03-13 - Upgrade jacoco plugin to 0.6.2 (from 0.6.1)
* 2013-03-13 - Upgrade dependency plugin to 2.7 (from 2.6)
* 2013-03-13 - Downgrade release plugin to 2.3.2 from 2.4 to fix git local checkout regression
* 2013-03-13 - Upgrade mavanagaiata plugin to 0.5.0 (from 0.4.1)
* 2013-03-13 - Upgrade maven-duplicate-finder-plugin to 1.0.4 (from 1.0.3)
* 2013-03-13 - Update guava dependency version to 14.0.1 (from 13.0.1)
* 2013-03-13 - Update objenesis version to 1.3 (from 1.2)
* 2013-03-13 - add build helper plugin 1.7
* 2013-03-13 - reactivate mavanagaiata plugin
* 2013-03-13 - ban guava versions before 10.0.1 (old r0<x> versions)
* 2013-03-13 - ban org.eclipse.jetty.orbit:javax.servlet (use javax.servlet:javax.servlet-api)
* 2013-03-13 - ignore duplicate classes in org.jruby:jruby-complete
* 2013-03-13 - make fork mode for surefire configurable using fb.test.fork-mode. Default is 'once' (was 'always').
* 2013-03-13 - skip mavanagaiata plugin when no git repository is present
* 2013-03-13 - skip existing license headers when running license:format
* 2013-03-13 - [BUG] fb.check.skip-jacoco did not do anything
* 2013-01-11 - make automatic push of release related files and tags configurable. Default is 'false' (was 'true').
* 2013-01-11 - Updated documentation.

----

## Version 4

* 2013-01-11 - Released
* 2013-01-11 - release plugin should only execute artifact deploy even if a site is present.

## Version 3

* 2013-01-11 - Released


