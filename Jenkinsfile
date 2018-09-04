podTemplate(
    label: 'mypod',
    inheritFrom: 'default',
    containers: [
        containerTemplate(
            name: 'helm',
            image: 'lachlanevenson/k8s-helm:v2.10.0',
            ttyEnabled: true,
            command: 'cat'
        )
    ],
) {
    node('mypod') {
        def commitId
        stage ('Extract') {
            checkout scm
            commitId = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
        }
        stage ('Deploy') {
            container ('helm') {
                sh "sed  -i 's#APPVERSION#'$env.BUILD_NUMBER'#' k8sadmin/Chart.yaml"
                sh "sed -i 's#CHARTVERSION#'$env.BUILD_NUMBER'#' k8sadmin/Chart.yaml"
                sh "helm init --client-only --skip-refresh"
                sh "helm upgrade --install k8sadmin k8sadmin/"
            }
        }
    }
}
