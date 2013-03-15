# Changes

## Version 5

* 2013-03-13 - Upgrade surefire plugin to 2.14 (from 2.13)
* 2013-03-13 - Upgrade pmd plugin to 3.0.1 (from 2.7.1)
* 2013-03-13 - Upgrade jacoco plugin to 0.6.2 (from 0.6.1)
* 2013-03-13 - Upgrade dependency plugin to 2.7 (from 2.6)
* 2013-03-13 - Downgrade release plugin to 2.3.2 from 2.4 to fix git local checkout regression
* 2013-03-13 - Upgrade mavanagaiata plugin to 0.5.0 (from 0.4.1)
* 2013-03-13 - Upgrade maven-duplicate-finder-plugin to 1.0.4 (from 1.0.3)
* 2013-03-13 - Update guava dependency version to 14.0 (from 13.0.1)
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


