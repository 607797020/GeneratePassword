pipeline {
  agent any
  stages {
    stage('Code check out or update') {
      steps {
        sh 'svn co https://collaborate.bt.com/svn/dnp-project/trunk/src/GeneratePassword --username 607797020 --password SilentQQWW89 --non-interactive'
      }
    }
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh 'mvn -f ./GeneratePassword/pom.xml clean install site'
          }
        }
        stage('Codesonar') {
          steps {
            sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.603:sonar -f ./GeneratePassword/pom.xml -Dsonar.projectKey=com.bt.genpass -Dsonar.login=admin -Dsonar.password=admin -Dsonar.language=java -Dsonar.sources=src/main/java -Dsonar.tests=src/test/java -Dsonar.test.inclusions=**/*Test*/** -Dsonar.exclusions=**com/bt/nat/capabilities/dnp/GenerateRandomPassword/ObjectFactory.java,**sep/bt/prf/generatepassword/util/Constants.java,**sep/bt/prf/generatepassword/util/BTLoggingConstants.java,**sep/bt/prf/generatepassword/data/oradata/*.java -Dsonar.scm.disabled=true -Dsonar.projectName=GeneratePassword -Dsonar.projectVersion=0.0.1 -Dsonar.sourceEncoding=UTF-8 -Dsonar.java.source=1.8 -Dsonar.dynamicAnalysis=reuseReports -Dsonar.java.binaries=target/classes -Dsonar.java.libraries=target/GeneratePasswordRS-V1/WEB-INF/lib -Dsonar.java.coveragePlugin=jacoco -Dsonar.junit.reportsPath=target/surefire-reports -Dsonar.jacoco.reportPaths=target/jacoco.exec -Dsonar.host.url=http://10.50.142.40:9090/sonar'
          }
        }
      }
    }
    stage('Codesonar') {
      steps {
        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.603:sonar -f ./GeneratePassword/pom.xml -Dsonar.projectKey=com.bt.genpass -Dsonar.login=admin -Dsonar.password=admin -Dsonar.language=java -Dsonar.sources=src/main/java -Dsonar.tests=src/test/java -Dsonar.test.inclusions=**/*Test*/** -Dsonar.exclusions=**com/bt/nat/capabilities/dnp/GenerateRandomPassword/ObjectFactory.java,**sep/bt/prf/generatepassword/util/Constants.java,**sep/bt/prf/generatepassword/util/BTLoggingConstants.java,**sep/bt/prf/generatepassword/data/oradata/*.java -Dsonar.scm.disabled=true -Dsonar.projectName=GeneratePassword -Dsonar.projectVersion=0.0.1 -Dsonar.sourceEncoding=UTF-8 -Dsonar.java.source=1.8 -Dsonar.dynamicAnalysis=reuseReports -Dsonar.java.binaries=target/classes -Dsonar.java.libraries=target/GeneratePasswordRS-V1/WEB-INF/lib -Dsonar.java.coveragePlugin=jacoco -Dsonar.junit.reportsPath=target/surefire-reports -Dsonar.jacoco.reportPaths=target/jacoco.exec -Dsonar.host.url=http://10.50.142.40:9090/sonar'
      }
    }
    stage('Deployment') {
      steps {
        sh 'java -cp /home/jenkins/wls/wlfullclient.jar weblogic.Deployer -debug -remote -verbose -noexit -name GeneratePassword -targets managed2_dnpwls01 -adminurl t3://172.17.0.2:61000 -userconfigfile /home/jenkins/wls/configuration.xml -user weblogic -password dnpwls02 -undeploy'
        sh 'java -cp /home/jenkins/wls/wlfullclient.jar weblogic.Deployer -debug -stage -remote -verbose -upload -name GeneratePassword -source ./GeneratePassword/target/GeneratePasswordRS-V1.war -targets managed2_dnpwls01 -adminurl t3://172.17.0.2:61000 -userconfigfile /home/jenkins/wls/configuration.xml -user weblogic -password dnpwls02 -deploy'
      }
    }
  }
}