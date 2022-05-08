pipeline {


  podTemplate(yaml: '''
  apiVersion: v1
  kind: Pod
  spec:
    containers:
    - name: docker
      image: docker:dind
      securityContext:
        privileged: true
      env:
        - name: DOCKER_TLS_CERTDIR
          value: ""
  ''') {
      node(POD_LABEL) {
          environment {
            HEROKU_API_KEY = credentials('adanisman-heroku-api-key')
          }
          container('docker') {
              sh 'docker build -t adanisman/springbootdocker:latest .'
              sh 'echo $HEROKU_API_KEY | docker login --username=_ --password-stdin registry.heroku.com'
              sh 'docker tag adanisman/springbootdocker:latest registry.heroku.com/$APP_NAME/web'
              sh 'docker push registry.heroku.com/adanisboot/web'
              sh 'heroku container:release web --app=adanisboot'

          }
      }
  }

}
