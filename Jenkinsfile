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

    stage("Generar tag"){
            steps {
                script {			
                    	//Generar tag

                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'sonarid',
                        usernameVariable: 'sonar', passwordVariable: 'GITHUB_ACCESS_TOKEN']]) {

                        echo "Usuario de GitHub: ${env.sonar}"
                        echo "Longitud del token: ${env.GITHUB_ACCESS_TOKEN?.length() ?: 'No asignado'}"
                        echo "Token del token: ${env.GITHUB_ACCESS_TOKEN}"
                    }

                    withCredentials([[$class: 'UsernamePasswordMultiBinding',credentialsId:'sonarid',usernameVariable:'sonar',
                    passwordVariable:'GITHUB_ACCESS_TOKEN']]){
                        gitTagPush(version:'1.0.0',
                        message:'descripcion tag',
                        username:env.sonar,
                        password:env.GITHUB_ACCESS_TOKEN,
                        email:"juang.rodriguez10@gmail.com")
                    }
                    echo "### Se termina ejecucion rama seleccionada #####"                 
                } //
            }
    }

    stage("seleccionar rama"){
            steps {
                script {			
                    def rama;
                    sh "git branch -a > ramas.txt"
                    sh "sed 's/remotes\\/origin\\///' ramas.txt > rama.txt"
                    sh "cat rama.txt"
                    ///Listar ramas
                    def contenido = sh(script: "cat rama.txt",returnStdout:true).trim()
                    echo contenido;
                    def values = contenido.split('\n')
                    ramasName=[]
                    for (String exp:values){
                        if(!exp.contains("master") &&  !exp.contains("HEAD")){
                            ramasName.add(exp);
                        }
                    }
                def RamaDeploy = input(
                    message: 'Selecciona:',
                    parameters: [
                        [$class: 'ChoiceParameterDefinition',
                            choices: ramasName.join('\n'),
                            name: 'Selecciona rama',
                            description: 'desc'
                        ]
                    ]
                )
                    echo "Rama : $RamaDeploy"
                    Branch=RamaDeploy.trim();
                    git branch: "${Branch}", credentialsId:'sonarid', url: 'https://github.com/juanRodriguezGarcia/cicd.git'
                    echo "Despues del cambio"
                    sh "git branch -a"
                    echo "##### Lectura del jenkinsfile de la rama #######################"
                    script{
                        load 'Jenkinfiledev'
                    }
                    echo "### Se termina ejecucion rama seleccionada #####"                 
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