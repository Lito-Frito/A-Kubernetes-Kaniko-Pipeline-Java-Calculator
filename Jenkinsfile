podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: gradle
        image: gradle:6.3-jdk14
        command:
        - sleep
        args:
        - 99d
        volumeMounts:
        - name: shared-storage
          mountPath: /mnt
      - name: kaniko
        image: gcr.io/kaniko-project/executor:debug
        command:
        - sleep
        args:
        - 9999999
        volumeMounts:
        - name: shared-storage
          mountPath: /mnt
        - name: kaniko-secret
          mountPath: /kaniko/.docker
      restartPolicy: Never
      volumes:
      - name: shared-storage
        persistentVolumeClaim:
          claimName: jenkins-pv-claim
      - name: kaniko-secret
        secret:
            secretName: dockercred
            items:
            - key: .dockerconfigjson
              path: config.json
''') {
  node(POD_LABEL) {
    stage('Run pipeline against a gradle project') {
        git branch: 'main', url: 'https://github.com/crc8109/week6'
        container('gradle') {

            stage('Build a gradle project!') {
                echo "I am the ${env.BRANCH_NAME} branch"
                sh '''
                pwd
                chmod +x gradlew
                ./gradlew test
                '''
                }

            stage("Code coverage!") {
                echo "CC is for Main branch; this is ${env.BRANCH_NAME}"
                if (env.BRANCH_NAME == "main") {
                    sh '''
                        pwd
                        ./gradlew jacocoTestReport
                        ./gradlew jacocoTestCoverageVerification
                    '''


                publishHTML (target: [
                    reportDir: 'build/reports/jacoco/test/html',
                    reportFiles: 'index.html',
                    reportName: "JaCoCo Report"
                ])
                }
            }

            stage("Static code analysis!") {
                echo "going to test statically now"
                sh '''
                pwd
                ./gradlew checkstyleMain
                '''
                publishHTML (target: [
                    reportDir: 'build/reports/checkstyle/',
                    reportFiles: 'main.html',
                    reportName: "Checkstyle Report"
                ])
            }
        }
  }

    stage('Build Java Image') {
      container('kaniko') {
        stage('Build a container') {
          sh '''
          echo 'FROM openjdk:8-jre' > Dockerfile
          echo 'COPY ./calculator-0.0.1-SNAPSHOT.jar app.jar' >> Dockerfile
          echo 'ENTRYPOINT ["java", "-jar", "app.jar"]' >> Dockerfile
          ls /mnt/*jar
          mv /mnt/calculator-0.0.1-SNAPSHOT.jar .
          /kaniko/executor --context `pwd` --destination crc8109/hello-kaniko:1.0
          '''
        }
      }
    }

  }
}
