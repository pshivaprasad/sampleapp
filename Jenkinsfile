// Uses Declarative syntax to run commands inside a container.
pipeline {
    agent {
        kubernetes {
            // Rather than inline YAML, in a multibranch Pipeline you could use: yamlFile 'jenkins-pod.yaml'
            // Or, to avoid YAML:
            // containerTemplate {
            //     name 'shell'
            //     image 'ubuntu'
            //     command 'sleep'
            //     args 'infinity'
            // }
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: registry.glams.com/glams/jenkins-agent:latest
    command:
    - sleep
    args:
    - infinity
    tty: true
    resources:
      requests:
        memory: "2Gi"
    volumeMounts:
     - name: docker-sock
       mountPath: /var/run/docker.sock
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
'''
            // Can also wrap individual steps:
            // container('shell') {
            //     sh 'hostname'
            // }
            defaultContainer 'shell'
        }
    }
    stages {
        stage('docker build') {
            steps {
                sh 'docker build -t alekhya277/sampleapp:latest .'
            }
        }
        stage('docker login') {
       steps {
            sh 'docker login -u alekhya277 -p Perigord!22'
        }
    }
    stage('docker push') {
        steps {
            sh 'docker push alekhya277/sampleapp'
           }
    }
        stage('deploy') {
            steps {
                sh 'kubectl apply -f Deployment.yaml'
            }
        }
    }
}
