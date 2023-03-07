#Prerequisites.
The Jenkins is installed and set up with ansible playbook from [project](https://github.com/Alliedium/awesome-jenkins/)
[multibranchpipelines](https://www.jenkins.io/doc/book/pipeline/multibranch/)

#Jenkinsfile description

Here we created Jenkinsfile to run our staged build on Jenkins. [How to run pipeline](https://www.jenkins.io/doc/book/pipeline/running-pipelines/)
We used declarative pipeline. 
Please see reference about [Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/) and [pipeline syntax](https://www.jenkins.io/doc/book/pipeline/syntax/) 
Our Jenkinsfile has 3 stages: _Build_, _Static code analysis_ and _Test_. 
We have 3 different maven [profiles](https://maven.apache.org/guides/introduction/introduction-to-profiles.html) in the pom.xml file, which are used for these three stages respectively: default, static-code-analysis and test

1. In the _Build_ stage maven dependencies are downloaded and jar file is build: 'mvn install -P default'

2. The static code analysis is performed during the _Static code analysis_ stage. 
We used [spotbugs](https://spotbugs.github.io/spotbugs-maven-plugin/), [pmd/cpd](https://pmd.github.io/latest/pmd_userdocs_tools_maven.html) and [checkstyle](https://checkstyle.org/) plugins. 
Analysis is run by maven command 'mvn -X install -P static-code-analysis'. In addition, we add checkstyle rules file as an option to our command.
We have 2 files with rules: sun_checks.xml - the standard file, downloaded from internet [github source](https://github.com/checkstyle/checkstyle/blob/52728d750690867f252e9312e1347a5b0010d9a4/src/main/resources/sun_checks.xml), checkstyle_checks.xml - with customized rules (here we changed maximum line length),
and 1 file with check suppression - we have suppressed Javadoc checks. 
You also can use [google style standards](https://github.com/checkstyle/checkstyle/tree/52728d750690867f252e9312e1347a5b0010d9a4/src/main/resources/google_checks.xml) checks or customize your own style checks.
In Jenkinsfile we added 4 options/combinations for codestyle checks which can be selected before the build: 'sun_checks', 'sun_checks_with_suppressions',
'custom_checks', 'custom_checks_with_suppressions'.
Static code analysis reports are published on Jenkins for all our tools using [WarningNG plugin](https://plugins.jenkins.io/warnings-ng/). 
We disabled build failure in the case if number of bugs or/and warnings will exceed predefined values by setting 'failOnError: false'. 
_**NB:** If you prefer to **fail** your build in the case of not passing static code analysis quality gate please set this parameter to **_true_**._ 
This parameter can be set separately for each of your tools

3. In the third stage _Test_ the unit tests are running and the test coverage report is provided. 
We used [Jacoco maven plugin](https://www.eclemma.org/jacoco/trunk/doc/maven.html) to do test coverage analysis. 
The report is published in Jenkins by the means of the [HTML publisher Jenkins plugin](https://plugins.jenkins.io/htmlpublisher/).

