import groovy.json.JsonSlurperClassic

def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}
pipeline {
  agent any 
  environment {
    AWS_REGION='us-east-1'
	AWS_DEFAULT_REGION='us-east-1'
  }
  stages {

    stage("paso 1"){
            steps {
                script {			
                sh "echo 'hola mundo desde GIT'"
                    echo "### antesd del cabmio de rama"
                    sh "pwd"
                    sh "ls -ltr"
                    sh "git branch -a"
                }
            }
    }

    stage("seleccionar rama"){
            steps {
                script {			
                    def rama;
                    sh "git branch -a > ramas.txt"
                    sh "sed 's/remotes\\/origin\\///' ramas.txt"
                    sh "cat ramas.txt"
                    ///Listar ramas
                    def contenido = sh(script: "cat ramas.txt",returnStdout:true).trim()
                    echo contenido;
                    def values = contenido.split('\n')
                    ramasName=[]
                    for (String exp:values){
                        if(!exp.contains("master") &&  !exp.contains("HEAD")){
                            ramasName.add(exp);
                        }
                    }


                    def RamaDeploy = input (message: 'Selecciona,',
                    parameter:[
                        [$class:'ChoiceParameterDefinition',
                        choices: ramasName.join('\n'),
                        name: 'Selecciona rama',
                        description: 'desc'
                        ]
                    ])
                    echo "Rama : $RamaDeploy"
                    Branch=RamaDeploy;
                    git branch: "${Branch}", credentialsId:'sonarid', url: 'https://github.com/juanRodriguezGarcia/cicd.git'
                    echo "Despues del cambio"
                    sh "git branch -a"
                    
                } //
            }
    }

  }
  post {
      always {          
          deleteDir()
           sh "echo 'ESTA FASE SIEMPRE SE EJECUTA SIN IMPORTAR SI FUE FALLIDO O NO'"
      }
      success {
            sh "echo 'ESTA FASE SE EJECUTA SOLAMENTE SI FUE EXITOSO'"
        }

      failure {
            sh "echo 'ESTA FASE SE EJECUTA SI FUE FALLIDO'"
      }
      
  }
} 