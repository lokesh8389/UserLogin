node{
    stage('UserLogin'){
        echo 'Hello World'
    }
    stage('SCM Checkout'){
        git 'https://github.com/lokesh8389/UserLogin'
    }
    stage('Compile-Package') {
        def mvnHome = tool name: 'Maven3', type: 'maven'
        sh "${mvnHome}/bin/mvn -version"
		sh "${mvnHome}/bin/mvn -Dmaven.test.failure.ignore=true clean deploy"
    }
	stage('sonar') {
	  
	    def scannerHome = tool 'sonar_scanner'
	    withSonarQubeEnv('sonar_server') {
            sh "${scannerHome}/bin/sonar-scanner"
	    }
	}    
    stage('ansible'){
	def proj_version = "0.0.1-SNAPSHOT"
	echo "${proj_version}"
	sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible_server', 
	transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: "ansible-playbook /etc/ansible/copywarUserLoginfile.yml -e extra_var=${proj_version}", 
	execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', 
	remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
    }
}
