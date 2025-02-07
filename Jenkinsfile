inode('linux'){

    
enviroment{
	AWS_REGION='us-east-1'
	AWS_DEFAULT_REGION='us-east-1'
}

stage('checkout'){
	echo "### antesd del cabmio de rama"
	sh "pwd"
	sh "ls -ltr"
	sh "git branch -a"
}

//Capaturar el ultimo comit
//sh "git rev-parse HEAD > commit"
//def commit = readFile('commit').trim()
//echo ${commit}



//echo "rama:  $scm.branches"
//def branch_name= scm.branches[0].name
//if(branch_name.contains("*/")){
//branch_name=branch_name.split("\\*/")[1]
//}
//echo "$branch_name"


stage("Deploy rama"){
	def rama;
	sh "git branch -a > ramas.txt"
	sh "sed 's/remotes\\/origin\\///' ramas.txt"
	sh "cat ramas.txt"
	///Listar ramas
	def contenido = sh(script: "cat ramas.txt",returnStdout:true).trim()
    echo contenido;
	def values = contenido.split('\n')
	ranasName=[]
	for (String exp:values){
		if(!exp.contains("master") &&  !exp.contains("HEAD")){
			ramasName.add(exp);
		}
	}
	
	def RamaDeploy = input (message: 'Selecciona,',
	parameter:[
		[$class:'ChoiseParameterDefinition',
		choices: ramasName.join('\n'),
		name: 'Selecciona rama',
		description: 'desc'
		]
	])
	echo "Rama : $RamaDeploy"
	Branch=RamaDeploy;
	git branch: "${Branch}", credentialsId:'Git-credential', url: https://repogit.git
	echo "Despues del cambio"
	sh "git branch -a"
	
	//Lectura del jenkins de la rama seleccionada
	script{
		load 'Jenkinsfile2'
	}
	
	
	//Generar tag
	withCredentials([[$class: 'UsernamePasswordMultiBinding',credentialsId:'git-hut-creden',usernameVariable:'GITHUB_APP',
	passeowdVariable:'GITHUB_ACESS_TOKEN']]){
		gitTagPush(version:'version1',
		message:'descripcion',
		username: env.GITHUB_APP,
		password:env.GITHUB_ACESS_TOKEN,
		email:"juang.rodriguez10@gmail.com")
	}
	
	

}




}






















