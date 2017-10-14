node {
    stage('checkout') {
        checkout scm
    }

    stage('ci') {
        // NOTE: please leave this as is if using tox
        sh 'echo $(mktemp -d 2>/dev/null || mktemp -d -t "mytmpdir") > toxworkdir'
        sh 'tox --workdir $(cat toxworkdir)'
        sh 'rm -rf $(cat toxworkdir)'  // cleanup
    }

    stage('publish') {
        junit '**/junit-*.xml'

        step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/coverage.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])

        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: false, reportDir: 'htmlcov', reportFiles: 'index.html', reportName: 'HTML Coverage Report', reportTitles: ''])

        step([
            $class                     : 'WarningsPublisher',
            parserConfigurations       : [[
                                              parserName: 'PYLint',
                                              pattern   : 'pylint.log'
                                         ]],
            unstableTotalAll           : '0',
            usePreviousBuildAsReference: true
        ])
    }
}


// node {
//     stage('checkout') {
//         checkout scm
//     }

//     stage('ci start') {

//         sh 'tox -e clean'

//         stash name: 'everything',
//             excludes: 'test-results/**',
//             includes: '**'

//     }
// }

// stage('tox tests') {
//     parallel check: {
//         runTox('check')
//     }, py27: {
//         runTox('py27')
//     }, py35: {
//         runTox('py35')
//     }, report: {
//         runTox('report')
//     }, docs: {
//         runTox('docs')
//     }
// }

// stage('publish') {
//     node {
//         junit '**/junit-*.xml'

//         step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/coverage.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])
//     }
// }

// def runTox(browser) {
//     node {
//         unstash 'everything'
//         sh "tox ${browser}"
//     }
// }
