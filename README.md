# docker-jenkins-gradle-html

### Usage
```bash
docker run -p 8080:8080 -p 50000:50000 \
-v jenkins:/var/jenkins_home \
-v html:/html \
desiato/jenkins-gradle-html
```

### sample jenkinsfile & nginx container
```groovy
node {
   stage('Preparation') {
      git 'https://github.com/hbdesiato/gradle-asciidoc-test.git'
   }
   stage('Build') {
       sh 'gradle asciidoctor'
   }
   stage('Results') {
       dir('build/asciidoc/html5') {
           archiveArtifacts '**'    
       }
       publishHTML(reportDir: 'build/asciidoc/html5', reportFiles: 'index.html', reportName: 'Asciidoctor Build')
       sh 'rm -rf /html/* && cp -r build/asciidoc/html5/* /html/'
   }
}
```

```bash
docker run -p 8081:80 -v html:/usr/share/nginx/html:ro nginx
```
