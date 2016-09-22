# code_analyzer
the tools we used to analyze our code

## how to use this project

add the project as a submodule, so that you can keep your resources up to date

`git submodule add https://github.com/reactivesw/code_analyzer_test.git`

use the fellow command to update the submodule

`git submodule foreach git pull`


# add your own config
add folder:`code_analyzer_test_local` to your project root, it contains two files:`code_analyzer.gradle`,`code_test_coverage.gradleadle`

## code_analyzer.gradle
```groovy

apply plugin: 'checkstyle'
apply plugin: 'pmd'
apply plugin: 'findbugs'

// apply all configurations
apply from:'code_analyzer_test/code_analyzer_config.gradle'

/*************checkstyle(use google java style)***************/

checkstyle{
    //exclude the package you do not want to check
    //checkstyleMain.exclude 'io/reactivesw/customerauthentication/grpc/*'
}

/*************PMD(Project Manager Design)***************/
tasks.withType(Pmd) {
    //exclude the package you do not want to check
    //exclude 'io/reactivesw/customerweb/Application.java'
}

/*************find bug***************/
tasks.withType(FindBugs) {
    //exclude the package you do not want to check
    /* findBugs doesn't work if the filter is empty. Comment all if nothing to exclude
    classes = classes.filter {
        !it.path.contains('io/reactivesw/customerauthentication/grpc/')
    }
    */
}
```

## code_unit_test.gradle
```groovy
// for spock unit test and code coverage

apply from:'code_analyzer_test/spock_spring_test.gradle'
apply from:'code_analyzer_test/unit_test_config.gradle'

//config of coverage check, see document: https://github.com/palantir/gradle-jacoco-coverage

jacocoCoverage {
    // Scopes can be exempt from all coverage requirements by exact scope name or scope name pattern.
    // fileThreshold 0.0, "Application.java"
    // packageThreshold 0.0, "org/company/module"
    // fileThreshold 0.0, ~".*Test.java"
}
```

# change build.gradle to apply code analyzer and test coverage

## Add External Dependency
Add `classpath 'com.palantir:jacoco-coverage:0.4.0'` in the `buildscript { dependencies {...}}` section. 
```groovy
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        // for unit test code coverage  -- need to find a way to put it in its file
        classpath 'com.palantir:jacoco-coverage:0.4.0'
    }
}
```

## Apply Analyzer and Code Coverage

Then apply the following two files to the late part of the `build.gradle` file -- better in the later section. 

```groovy 
apply from: 'code_analyzer_test_local/code_analyzer.gradle'
apply from: 'code_analyzer_test_local/code_test_coverage.gradle'
```