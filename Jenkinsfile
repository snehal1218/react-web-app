pipeline
{
 agent any
  
 tools
  {
    nodejs 'node' 
    
  }

 stages
  {
   stage('Build')
    {
     steps
      {
       script
        {
         cleanWs()
         checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '${RepoBranch}']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url:'https://github.com/roshnirpawar7555/react-web-app.git']]] 
         sh '''
         ls -ltr
         npm install
         npm run build
         ls -ltr
         cd build/
         tar -cvf frontend-${BUILD_NUMBER}.tar *
         cp frontend-${BUILD_NUMBER}.tar ${WORKSPACE}/
         ls -ltr
         '''
          
        
        }
      }
     
    }
  stage('Deploy')
   {
    steps
    {
     sh '''
     rm -rf deploy
     mkdir deploy
     cp -r frontend-${BUILD_NUMBER}.tar deploy/
     cd deploy
     tar -xvf frontend-${BUILD_NUMBER}.tar
     rm -rf frontend-${BUILD_NUMBER}.tar
     ls -ltr
     #gsutil acl ch -u AllUsers:R gs://react-web-app2
     #gsutil defacl set public-read gs://react-web-app2
     rm -rf ~/.gsutil
     gsutil web set -m index.html -e index.html gs://react-web-app2
     
     gsutil cp -r * gs://react-web-app2
     gsutil setmeta -h "content-type: image/svg+xml" gs://react-web-app1/static/media/*.svg
     
      '''
    } 
   } 
    
  }  
  
}  
