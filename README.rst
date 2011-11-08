This code is a quick hack that analyzes Debian sid piuparts_ packages that are in state-dependency-failed-testing_, and prints out information on the failed packages which are blocking their testing.

.. _piuparts: http://piuparts.debian.org/
.. _state-dependency-failed-testing: http://piuparts.debian.org/sid/state-dependency-failed-testing.html

The output consists of:

* the number of packages in state-failed-dependency-testing which have been traced to a state-failed-testing package
* the number of packages in state-failed-testing causing the dependencies
* the list of state-failed-testing packages sorted by impact, with:

 - the number of packages blocked by this package
 - the number of packages which are blocked only by this package
 - the number of packages left in state-failed-dependency-testing after this package and the ones above it pass
 - the total number of packages which depend on this package, directly or recursively

Example::

    # ./piublocker 
    dependency failed -  2047
    failed testing -  222
    
    blocking free cum  rdeps package
     458      39  2008   497 sgml-data
     439       3  1613   477 docbook-xsl
     278     202  1411   706 ca-certificates-java
     199     182  1221   205 texlive-base
     119     119  1102   163 iceweasel
      91      13  1088   121 menu
      88      29  1059    93 gnustep-base-common
      69       1   990    69 blends-common
      59       0   931    60 gnustep-back0.20
      58      10   896    85 antlr
      55      36   857   714 libhttp-date-perl
      55       5   825   124 libcommons-httpclient-java
      47      13   802   118 libcommons-beanutils-java
      45      23   778   117 openssh-client
      36      15   749    36 gforge-common
      35      34   714    35 gosa
      33      33   681    33 liquidsoap
      30       3   678    30 libspring-core-java
      28       1   656    28 libatinject-jsr330-api-java
      26      26   630    27 drupal6

The cumulative effect of removing the top blocking failed packages on this date can be seen here (from an earlier run):



.. image:: piublocker/raw/master/images/Screenshot.png

This is a typical distribution of packages states in Debian sid piuparts. The packages in state-dependency-failed-testing are in dark red (from http://piuparts.debian.org/sid/)

.. image:: piublocker/raw/master/images/states.png

The application parses http://piuparts.debian.org/sid/state-dependency-failed-testing.html to gather package data. The information is stored locally in the file piudata.json, to speed up subsequent runs. Delete the file to cause the data to be downloaded again.

`BeautifulSoup <http://www.crummy.com/software/BeautifulSoup/>`_ and apt-rdepends must be installed.

The number of packages counted in state-failed-dependency-testing is less than that reported by the web page, possibly because it only counts packages which trace directly to a failed-dependency-test package.

The code is not a model of cleanliness, or efficiency.

.. David Steele



