@Library('global-library-test@main') _

import groovy.json.JsonOutput
import firm.project.Env

def configs = Env.getEnviroments(env)

node {

    env.DEBIAN_FRONTEND = 'noninteractive'
    env.TZ = 'Europe/Istanbul'

    docker.image("instrumentisto/flutter:3.19.6-androidsdk34-r0").inside("--user 0 --volume=$HOME//.ssh://root//.ssh -v .://app -w //app instrumentisto/flutter \\") {
        stage('CHECKOUT') {
            gitCheckoutLibrary(configs.git)
        }
        stage('INSTALL TOOLS') {
            installDependenciesLibrary(configs.dockerImage.dependencies)
        }
        stage('INITIALIZE') {
            flutterAnalyzeLibrary(configs.flutter.analyzeOptions)
        }
        stage('TEST') {
            flutterTestLibrary()
        }
        stage('BUILD') {
            flutterBuildLibrary()
        }
        stage('UPLOAD ARTIFACT') {
            zipTheFileLibrary(configs.zipFile)

            //uploadToNexusLibrary(configs.nexus)
        }
        stage('PUBLISH') {
            publishAndroidLibrary(configs.googlePlay)
        }
    }
}