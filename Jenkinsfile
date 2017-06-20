#!groovy

@Library('Reform')
import uk.gov.hmcts.Artifactory
import uk.gov.hmcts.Packager
import uk.gov.hmcts.RPMTagger

def artifactory = new Artifactory(this)
def packager = new Packager(this, 'cc')

properties([
        [$class: 'GithubProjectProperty', displayName: 'payment-api promotion pipeline', projectUrlStr: 'https://git.reform.hmcts.net/common-components/payment-promotion-pipeline'],
        parameters([string(description: 'RPM Version', name: 'rpmVersion')])
])

node {
    try {
        stage('Tag testing passed'){
            new RPMTagger(this, 'payment-api', packager.rpmName('payment-api', params.rpmVersion), 'cc-local').tagTestingPassedOn('test')
        }

        stage('Promote to Prod') {
            deleteDir()
            artifactory.promoteLatestEligibleRPMtoProductionRepo('cc', 'payment-api')
        }
    } catch (Throwable err) {
        notifyBuildFailure channel: '#cc_tech'
        throw err
    }
}