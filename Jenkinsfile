def jobName = JOB_NAME
def projectName = jobName.split('/')[0]
def mvn_version = 'Maven'  
node{
    stage('Checkout'){
                 echo  "Build: ${projectName} for branch ${BRANCH_NAME}"
                 git(	
                 url: "https://github.com/Sai6696/First-Project.git",
				 credentialsId: 'Github',               
				 branch: "${BRANCH_NAME}"				 
				)
			bat 'git clean -f'		            
			bat 'git reset --hard'              
			bat 'git checkout .'             
    }
    stage('Build'){
                 echo "echo Building ${BRANCH_NAME}..."
                 bat "mvn clean install -DskipTests=true"
    }
    stage('Test'){
                 echo "echo Testing ${BRANCH_NAME}..."
                 bat "mvn clean test"
      }
    stage('Deploy'){
                 if("${BRANCH_NAME}" == 'develop'){
                    echo "echo Deploying to ${BRANCH_NAME}..."
                    withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                        echo "${tool mvn_version}"
                        bat "mvn clean deploy -Denvironment= "DEV" -DmuleDeploy"
                    }
                }
                else if("${BRANCH_NAME}" == 'qa'){
                    echo "echo Deploying to ${BRANCH_NAME}..."
                    withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                        echo "${tool mvn_version}"
                        def pom = readMavenPom file: 'pom.xml'
                        def versionList = pom.version.replace("-SNAPSHOT", "").tokenize(".")
						version = "${versionList[0]}.${versionList[1]}.${versionList[2]}"
                        bat "mvn clean deploy -Denvironment='QA' -DmuleDeploy"
                    }
                }
                else if("${BRANCH_NAME}" == 'sit'){
                    echo "echo Deploying to ${BRANCH_NAME}..."
                    withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                        echo "${tool mvn_version}"
                        def pom = readMavenPom file: "${projectName}/pom.xml"
                        def versionList = pom.version.replace("-SNAPSHOT", "").tokenize(".")
						version = "${versionList[0]}.${versionList[1]}.${versionList[2]}"
                        bat "mvn clean deploy -Denvironment='SIT' -DmuleDeploy"
                    }
                }
                else if("${BRANCH_NAME}" == 'master'){
                    echo "echo Deploying to ${BRANCH_NAME}..."
                    withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                        echo "${tool mvn_version}"
                        def pom = readMavenPom file: "${projectName}/pom.xml"
                        def versionList = pom.version.replace("-SNAPSHOT", "").tokenize(".")
						version = "${versionList[0]}.${versionList[1]}.${versionList[2]}"
                        bat "mvn clean deploy -Denvironment='PROD' -DmuleDeploy"
                    }
                }             
            
    }
}