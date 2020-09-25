pipeline {
    agent any
    stages {
         stage('Retrieve Sources') {
              steps {
         echo workspace
         git url: 'https://github.com/JoeOffenberg/TradeMax.git'
         sh "ls -la"
    	}
    }
      stage ('Validation'){
                                 
        parallel {
        
        stage ('Config'){
		stages ('Sweagle Steps'){
		           
		       

        stage('UploadConfig'){
        
            steps {
                
                SWEAGLEUpload(
                actionName: 'Upload Config Files', 
                fileLocation: "WebContent/META-INF/my.cnf", 
                format: 'INI', 
                markFailed: false, 
                nodePath: 'infra,db023,mycnf', 
                onlyParent: false, 
                showResults: false,
                withSnapshot: false,
                description: 'Upload MySQL config',
                tag: '', 
                autoRecognize: true,
                allowDelete: false)
                
                SWEAGLEUpload(
                actionName: 'Upload JSON Files', 
                fileLocation: "*.json", 
                format: 'json', 
                markFailed: false, 
                nodePath: 'Applications,TradeMax,Files', 
                onlyParent: false, 
                showResults: false,
                withSnapshot: false,
                subDirectories: true,
                description: 'Upload json files',
                tag: '', 
                autoRecognize: false,
                allowDelete: false)

            }
        }
        
            stage('Validate Config') {
                steps {
                    SWEAGLEValidate(
                    actionName: 'Validate Config Files',
                    mdsName: 'TradeMax-PRD',
                    warnMax: -1,
                    errMax: 0,
                    markFailed: true,
                    showResults: true, 
                    retryCount: 5,
                    retryInterval: 30)
                    }
            	}	
            
            
       
                
            
        stage('Snapshot Config') {
            steps {
              SWEAGLESnapshot(
              actionName: 'Validated Snapshot TradeMax-PRD',
              mdsName: 'TradeMax-PRD',
              description: "Validated Snapshot for Jenkins Build ${BUILD_ID}",
              tag: "Version:1.7.${BUILD_ID}",
              markFailed: false,
              showResults: false)
              
              
            }
        }
        
        stage('Export Config') {
            steps {
              SWEAGLEExport(
              actionName: 'Export TradeMax-PRD settings.json',
              mdsName: 'TradeMax-PRD',
              exporter: 'returnDataforNode',
              args: "mycnf",
              format: 'json',
              fileLocation: "settings.json",
              markFailed: true,
              showResults: true)
              
              
            }
        }
			}
			} //Sweagle versioining and validation
			
		stage ('Code'){ 
		stages{
    			
			    stage('jUnit Test'){ 
                steps {echo "Testing..."
                     }
                  }
                  
                stage('SonarQube'){ 
                steps {sh 'sonar-scanner 55'
                     }
                  }
                  
                  }	
                 
                  
        }
       } //parallel
    } //Validation Stage
    
    
    stage ('Build'){
    steps {sleep(time:35,unit:"SECONDS")
                     }
    
    }
    
    stage ('Deployment'){
    steps {sleep(time:35,unit:"SECONDS")
                     }
    
    }
    
    stage (Functional) {
        parallel {
       	          			
			    stage('Selenium API'){ 
                steps { echo "Selenium API..2..3..4"
                		sleep(time:25,unit:"SECONDS")
                		echo "Selenium API..2..3..4"
                     }
                  }
                  
                stage('Selenium UI'){ 
                steps {	echo "Selenium UI..2..3..4"
                		sh 'selenium 35'
                		echo "Selenium API..2..3..4"
                     }
                  }
                 
    }
    
    }//Functional Testing
    
 } //Outer Stages
} //Pipeline
