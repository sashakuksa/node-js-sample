pipeline {
agent any
environment {
NODE_ENV = 'development'
}
stages {
stage('Checkout') {
steps {
git branch: "${env.BRANCH_NAME}", url: 'https://github.com/sashakuksa/node-js-sample'
}
}
stage('Install Dependencies') {
steps {
script {
if (fileExists('package.json')) {
sh 'npm install'
} else {
error 'No package.json found'
}
}
}
}
stage('Build') {
steps {
sh 'npm run build || echo "No build script found"'
}
}
stage('Test') {
steps {
sh 'npm test'
}
post {
always {
junit 'reports/test-results.xml'
archiveArtifacts artifacts: 'reports/**', allowEmptyArchive: true
}
}
}
stage('Deploy') {
when {
branch 'master'
}
environment {
NODE_ENV = 'production'
}
steps {
sh 'npm run deploy-prod || echo "No deploy-prod script found"'
}
}
stage('Deploy1') {
when {
branch 'dev'
}
steps {
sh 'npm run deploy-dev || echo "No deploy-dev script found"'
}
}
}
post {
always {
echo 'Pipeline finished.'
}
success {
mail to: 'team@example.com',
subject: "SUCCESS: ${currentBuild.fullDisplayName}",
body: "Good news! The build ${currentBuild.fullDisplayName} completed successfully."
}
failure {
mail to: 'team@example.com',
subject: "FAILURE: ${currentBuild.fullDisplayName}",
body: "Bad news! The build ${currentBuild.fullDisplayName} failed. Please check the Jenkins logs for more details."
}
}
}
