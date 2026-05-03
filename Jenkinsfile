node {
    docker.image('python:3.9-slim').inside {
        try {
            stage('Build') {
                sh 'python3 -m py_compile sources/add2vals.py sources/calc.py'
            }
            stage('Test') {
                sh 'pip install pytest && py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
                junit 'test-reports/results.xml'
            }

            stage('Deliver') {
                sh 'pip install pyinstaller && pyinstaller --onefile sources/add2vals.py'
                archiveArtifacts 'dist/add2vals'
            }
        } catch (exc) {
            currentBuild.result = 'FAILURE'
            throw exc
        }
    }
}