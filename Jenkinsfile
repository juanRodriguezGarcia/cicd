import groovy.json.JsonSlurperClassic

def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}
pipeline {
  agent any 
  environment {
    AWS_REGION='us-east-1'
	AWS_DEFAULT_REGION='us-east-1'
	GH_TOKEN = credentials('gitsonar') // Usa el ID de la credencial de Jenkins
  }
  stages {

    stage("paso 1"){
            steps {
                script {			
                sh "echo 'hola mundo desde GIT'"
                echo "####################################### GH ####################################"
                sh 'echo $PATH'
                sh 'gh --version'
                sh 'echo "Token: ${GH_TOKEN:0:4}********"'
                    echo "### antesd del cabmio de rama"
                    sh "pwd"
                    sh "ls -ltr"
                    sh "git branch -a"
                    
                }
            }
    }

    // //Desplega una rama remota y genera un tag de la rama remota indicada
    // stage("Desplegar rama remota"){
    //         steps {
    //             script {			
    //                 sh "pwd"
    //                 sh "ls -ltrh"
    //                 def rama;
    //                 echo "### ramas remotas"
    //                 sh "rm -rf ramas.txt"
                
    //                 //git ls-remote --heads https://github.com/juanRodriguezGarcia/fuentes.git | cut -f2 | cut -d'/' -f3
    //                 //sh "git ls-remote --heads https://github.com/juanRodriguezGarcia/fuentes.git | awk '{print $2}' | sed 's|refs/heads/||' > ramas.txt"
    //                 sh "git ls-remote --heads https://github.com/juanRodriguezGarcia/fuentes.git | cut -f2 | cut -d'/' -f3 > rama.txt"                  
    //                 sh "cat rama.txt"
    //                 ///Listar ramas
    //                 def contenido = sh(script: "cat rama.txt",returnStdout:true).trim()
    //                 echo contenido;
    //                 def values = contenido.split('\n')
    //                 ramasName=[]
    //                 for (String exp:values){
    //                     if(!exp.contains("master") &&  !exp.contains("HEAD")){
    //                         ramasName.add(exp);
    //                     }
    //                 }
    //                 def RamaDeploy = input(
    //                     message: 'Selecciona:',
    //                     parameters: [
    //                         [$class: 'ChoiceParameterDefinition',
    //                             choices: ramasName.join('\n'),
    //                             name: 'Selecciona rama',
    //                             description: 'desc'
    //                         ]
    //                     ]
    //                 )
    //                 echo "Rama : $RamaDeploy"
    //                 Branch=RamaDeploy.trim();
    //                 //La credencial sonarid se configura en credential de jenkins tipo Username with password para los Pull request se usa tipo Secret Text
    //                 //git branch: "${Branch}", credentialsId:'sonarid', url: 'https://github.com/juanRodriguezGarcia/cicd.git'
    //                 git branch: "${Branch}", credentialsId:'sonarid', url: 'https://github.com/juanRodriguezGarcia/fuentes.git'
    //                 echo "Despues del cambio de repositorio remoto"
    //                 sh "git branch -a"
    //                 echo "##### Lectura del jenkinsfile de la rama (*-*) $Branch #######################"
    //                 script{
    //                     load 'Jenkinfiledev'
    //                 }
    //                 //usernameVariable: 'sonar'  sonar es el valor configurado en la llave sonarid esto retorna un token en la variable global de jenkins GITHUB_ACCESS_TOKEN
    //                 echo "##### Vuelve a la rama indicada para generar el tag a partir de esa rama #################"
    //                     withCredentials([usernamePassword(credentialsId: 'sonarid', usernameVariable: 'sonar',  passwordVariable: 'GITHUB_ACCESS_TOKEN')]) {
    //                     script {
    //                         echo "Usuario de GitHub: ${env.sonar}"
    //                         echo "Longitud del token: ${env.GITHUB_ACCESS_TOKEN.length()}"                   
    //                         // Checkout con Jenkins
    //                         //git branch: Branch, credentialsId: 'sonarid', url: 'https://github.com/juanRodriguezGarcia/cicd.git'
    //                         git branch: Branch, credentialsId: 'sonarid', url: 'https://github.com/juanRodriguezGarcia/fuentes.git'
    //                         sh "ls -ltr"
    //                         echo "##Valor token###"
    //                         sh 'echo "Token: $GITHUB_ACCESS_TOKEN"'
                            
    //                         sh '''
    //                         git config --global user.email "juang.rodriguez10@gmail.com"
    //                         git config --global user.name "$sonar"
                               
    //                         # Cambiar la URL remota para autenticarse con el token
    //                         git remote set-url origin https://sonar:$GITHUB_ACCESS_TOKEN@github.com/juanRodriguezGarcia/fuentes.git
    //                         git tag
    //                         git tag -l | xargs git tag -d  # Borra todos los tags locales
    //                         #git ls-remote --tags origin | awk '{print ":" $2}' | xargs git push origin
    //                         git tag
    //                         #git tag -d 1.0.0 //borra un tag en especifico
    //                         pwd
    //                         ls -ltrh
    //                         git remote -v
    //                         git branch
    //                         git tag -a 1.0.9 -m "Rama $Branch version 9"
    //                         git push origin --tags
    //                         git tag
    //                         '''
    //                     }
    //                 }
                    
    //                 echo "### Se termina ejecucion rama seleccionada #####"                 
    //             } //
    //         }
    // }



        //Genera un Pull request a una repo remoto desde un tag del repo remoto creado previamente
        stage("paso crear rama desde TAG y crear PR para mergear a main o master desde dicha rama nuevan"){
            steps {
                script {			
                    echo "### Se va a generar una rama temporal a partir de un tag para mergerarlo a master y develop ################"
                       //GITHUB_ACCESS_TOKEN  es una variable que retorna el comando withCredentials  que trea e token seguro configurado antes en jenkins
                       //en la url https://sonar sonar es el usuario configurado en jenkins en la credencialid sonarid
                       withCredentials([usernamePassword(credentialsId: 'sonarid', usernameVariable: 'sonar', passwordVariable: 'GITHUB_ACCESS_TOKEN')]) {
                           sh """
                           git remote set-url origin https://sonar:$GITHUB_ACCESS_TOKEN@github.com/juanRodriguezGarcia/fuentes.git
                           git fetch --tags
                           git checkout -b release-1.0.10 1.0.10
                           git push origin release-1.0.10
                           """
                       }
                       //Para los Pull request se debe tener autenticado un token con permisos previamente puede ser desde la ec2 directamente agregando un comando como e siguiente:
                       //echo "toknegit_F4ZGOcdRxiQxmy52h9token" | gh auth login --with-token     
                        withCredentials([
                            usernamePassword(credentialsId: 'sonarid', usernameVariable: 'sonar', passwordVariable: 'GITHUB_ACCESS_TOKEN'),
                            string(credentialsId: 'gitsonar', variable: 'GH_TOKEN')
                        ]) {
                            sh """
                            # Configurar autenticación en GitHub CLI
                            #echo \$GH_TOKEN | gh auth login --with-token
                            # Crear un archivo de prueba para que la rama tenga cambios si se desea agregar al release
                            #echo "Actualización desde Jenkins" > update.txt
                            #git add update.txt
                            #git commit -m "Agregando un cambio para PR desde Jenkins"
                            #git push origin featur-1.0.1
                            pwd
                            ls -ltrh
                            git remote -v
                            git branch
                            # Crear PR desde la nueva rama
                               gh pr create --base main --head release-1.0.10 --title "Release a prod desde el tag 1.0.10 fuentes prod" --body "Este PR es productivo"
                            """
                        }  
                       try{
                        withCredentials([
                            usernamePassword(credentialsId: 'sonarid', usernameVariable: 'sonar', passwordVariable: 'GITHUB_ACCESS_TOKEN'),
                            string(credentialsId: 'gitsonar', variable: 'GH_TOKEN')
                        ]) {
                            sh """
                            pwd
                            ls -ltrh
                            git remote -v
                            git branch
                            # Crear PR desde la nueva rama
                               gh pr create --base develop --head release-1.0.10 --title "Release a prod desde el tag 1.0.10 fuentes dev " --body "Este PR es desarrollo"
                            """
                        } 
                       }catch(Exception ){
                            echo "Error pr develop"
                       }
                }//
            }
    }



//     stage("paso crear rama desde TAG y crear PR para mergear a main o master desde dicha rama nuevan"){
//             steps {
//                 script {			
//                     echo "### Se va a generar una rama temporal a partir de un tag para mergerarlo a master y develop ################"
// //                        //GITHUB_ACCESS_TOKEN  es una variable que retorna el comando withCredentials  que trea e token seguro configurado antes en jenkins
// //                        withCredentials([usernamePassword(credentialsId: 'sonarid', usernameVariable: 'sonar', passwordVariable: 'GITHUB_ACCESS_TOKEN')]) {
// //                            sh """
// //                            git fetch --tags
// //                            git checkout -b featu-1.0.0 1.0.0
// //                            git remote set-url origin https://sonar:$GITHUB_ACCESS_TOKEN@github.com/juanRodriguezGarcia/cicd.git
// //                            git push origin featu-1.0.0
// //                            """
// //                        }
                        
//                         withCredentials([
//                             usernamePassword(credentialsId: 'sonarid', usernameVariable: 'sonar', passwordVariable: 'GITHUB_ACCESS_TOKEN'),
//                             string(credentialsId: 'gitsonar', variable: 'GH_TOKEN')
//                         ]) {
//                             sh """
//                             #git fetch --tags
//                             #git checkout -b featur-1.0.1 1.0.1
                            
//                             # Configurar autenticación en GitHub CLI
//                             #echo \$GH_TOKEN | gh auth login --with-token
//                             # Crear un archivo de prueba para que la rama tenga cambios
//                             #echo "Actualización desde Jenkins" > update.txt
//                             #git add update.txt
//                             #git commit -m "Agregando un cambio para PR desde Jenkins"
//                             #git push origin featur-1.0.1
                        
//                             # Crear PR desde la nueva rama
//                             #gh pr create --base main --head featu-1.0.0 --title "Feature desde el tag 1.0.0" --body "Este PR corrige XYZ"
//                             gh pr create --base develop --head featu-1.0.0 --title "Feature desde el tag 1.0.0" --body "Este PR corrige XYZ"
//                             """
//                         }       
//                 }//
//             }
//     }

    //desplegar una rama del mismo repo y generar tag de una rama del mismo repo
    // stage("seleccionar rama"){
    //         steps {
    //             script {			
    //                 def rama;
    //                 sh "git branch -a > ramas.txt"
    //                 sh "sed 's/remotes\\/origin\\///' ramas.txt > rama.txt"
    //                 sh "cat rama.txt"
    //                 ///Listar ramas
    //                 def contenido = sh(script: "cat rama.txt",returnStdout:true).trim()
    //                 echo contenido;
    //                 def values = contenido.split('\n')
    //                 ramasName=[]
    //                 for (String exp:values){
    //                     if(!exp.contains("master") &&  !exp.contains("HEAD")){
    //                         ramasName.add(exp);
    //                     }
    //                 }
    //             def RamaDeploy = input(
    //                 message: 'Selecciona:',
    //                 parameters: [
    //                     [$class: 'ChoiceParameterDefinition',
    //                         choices: ramasName.join('\n'),
    //                         name: 'Selecciona rama',
    //                         description: 'desc'
    //                     ]
    //                 ]
    //             )
    //                 echo "Rama : $RamaDeploy"
    //                 Branch=RamaDeploy.trim();
    //                 git branch: "${Branch}", credentialsId:'sonarid', url: 'https://github.com/juanRodriguezGarcia/cicd.git'
  
                    
    //                 echo "Despues del cambio"
    //                 sh "git branch -a"
    //                 echo "##### Lectura del jenkinsfile de la rama #######################"
    //                 script{
    //                     load 'Jenkinfiledev'
    //                 }
                    
    //                 echo "##### Vuelve a la rama indicada para generar el tag a partir de esa rama #################"
    //                     withCredentials([usernamePassword(credentialsId: 'sonarid', usernameVariable: 'sonar',  passwordVariable: 'GITHUB_ACCESS_TOKEN')]) {
    //                     script {
    //                         echo "Usuario de GitHub: ${env.sonar}"
    //                         echo "Longitud del token: ${env.GITHUB_ACCESS_TOKEN.length()}"
                    
    //                         // Checkout con Jenkins
    //                         git branch: Branch, credentialsId: 'sonarid', url: 'https://github.com/juanRodriguezGarcia/cicd.git'
                    
    //                         echo "##Valor token###"
    //                         sh 'echo "Token: $GITHUB_ACCESS_TOKEN"'
                            
    //                         sh '''
    //                         git config --global user.email "juang.rodriguez10@gmail.com"
    //                         git config --global user.name "$sonar"
                            
    //                         # Cambiar la URL remota para autenticarse con el token
    //                         git remote set-url origin https://sonar:$GITHUB_ACCESS_TOKEN@github.com/juanRodriguezGarcia/cicd.git
 
    //                         git tag -a 1.0.1 -m "Rama $branchName"
    //                         git push origin --tags
    //                         '''
    //                     }
    //                 }
                    
    //                 echo "### Se termina ejecucion rama seleccionada #####"                 
    //             } //
    //         }
    // }
    
    



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


    //     stage("Generar tag desde la misma rama sobre la que se esta trabajando"){
    //         steps {
    //             script {			
    //                 	//Generar tag

    //                 withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'sonarid',
    //                     usernameVariable: 'sonar', passwordVariable: 'GITHUB_ACCESS_TOKEN']]) {

    //                     echo "Usuario de GitHub: ${env.sonar}"
    //                     echo "Longitud del token: ${env.GITHUB_ACCESS_TOKEN?.length() ?: 'No asignado'}"
    //                     echo "Token del token: ${env.GITHUB_ACCESS_TOKEN}"
    //                 }

    //                 withCredentials([usernamePassword(credentialsId: 'sonarid', 
    //                                                   usernameVariable: 'sonar', 
    //                                                   passwordVariable: 'GITHUB_ACCESS_TOKEN')]) {
    //                     script {
    //                         echo "Usuario de GitHub: ${env.sonar}"
    //                         echo "Longitud del token: ${env.GITHUB_ACCESS_TOKEN.length()}"
                    
    //                         sh '''
    //                         git config --global user.email "juang.rodriguez10@gmail.com"
    //                         git config --global user.name "$sonar"
    //                         git remote set-url origin https://$GITHUB_ACCESS_TOKEN@github.com/juanRodriguezGarcia/cicd.git
    //                         git tag -a version1 -m "descripcion"
    //                         git push origin --tags
    //                         '''
    //                     }
    //                 }
    //                 echo "### Se termina ejecucion rama seleccionada #####"                 
    //             } //
    //         }
    // }