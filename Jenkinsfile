node ("Dev-Slave"){

   // pull request or feature branch
    if  (env.BRANCH_NAME != 'master') {
        checkout()
        build()
        //unitTest()
        // test whether this is a regular branch build or a merged PR build
        if (!isPRMergeBuild()) {
           sh "exit 1"
        } else {
            // Pull request
        }
    } // master branch / production
    else { 
        checkout()
        build()
		test()
		push()
    }
}

def isPRMergeBuild() {
    return (env.BRANCH_NAME ==~ /^PR-\d+$/)
}
def build () {
     stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        docker.build("bhargav7/techgig-docker")
		}
	}
	
def checkout () {
    stage 'Checkout code'
    context="continuous-integration/jenkins/"
    context += isPRMergeBuild()?"pr-merge/checkout":"branch/checkout"
    checkout scm
    setBuildStatus ("${context}", 'Checking out completed', 'SUCCESS')
	}
	
def test (){

    stage 'Test image' 
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

    }
	
def push (){
    stage 'Push image'
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
		push("${env.BUILD_NUMBER}")
        push("latest")
		}
    }
