@Library('global-library-test@main') _

import groovy.json.JsonOutput
import firm.project.Env

def configs = Env.getEnviroments(env)

node {

    env.DEBIAN_FRONTEND = 'noninteractive'
    env.TZ = 'Europe/Istanbul'
     properties([
         parameters([
           booleanParam(
             defaultValue: false,
             description: 'isFoo should be false',
             name: 'isFoo'
           ),
           booleanParam(
             defaultValue: true,
             description: 'isBar should be true',
             name: 'isBar'
           ),
         ])
       ])
    docker.image("instrumentisto/flutter:3.19.6-androidsdk34-r0").inside("--user 0 --volume=\$HOME/.ssh:/root/.ssh -v .:/app -w /app") {
                    stage('CHECKOUT') {
                        gitCheckoutLibrary(configs.git)
                    }
                    stage('INSTALL TOOLS') {
                        installDependenciesLibrary(configs.dockerImage.dependencies)
                    }
                    stage('ANALYZE') {
                        flutterAnalyzeLibrary(configs.flutter.analyzeOptions)
                    }
                    stage('TEST') {
                        flutterTestLibrary()
                    }
            //         stage('BUILD') {
            //             flutterBuildLibrary()
            //         }
            //         stage('UPLOAD ARTIFACT') {
            //             zipTheFileLibrary(configs.zipFile)
            //
            //             //uploadToNexusLibrary(configs.nexus)
            //         }
                    stage('PUBLISH') {
                        when{
                           expression{
                                BRANCH_NAME == 'master'
                           }
                        }
                        publishAndroidLibrary(configs.googlePlay)
                    }
        post{
            always{
                    echo "Always"
            }
            failure{
            }
        }
    }
}