# helm-bugs

## ** Error: UPGRADE FAILED: another operation (install/upgrade/rollback) is in progress **
## Pending-upgrade / pending-install

Using jenkins pipeline:  if there is a helm release in the pending state AND it has more than one revision, then rollback to the previous revision 
Otherwise, if there is a helm release in the state pending AND it has only one revision, then remove that release
*
 STATUS = sh (
                script: "helm status '$projectName' | grep STATUS: | cut -d ':' -f2 ",
                returnStdout: true
            ).trim()
        echo "Status: ${STATUS}"
            REVISON = sh (
                script: "helm status '$projectName' | grep REVISION: | cut -d ':' -f2 ",
                returnStdout: true
            ).trim()
        echo "REVISION: ${REVISON}"
        
        //sh "echo Revison Number is ${REVISON-NUMBER}"
        if("$STATUS" ==~ "(pending).*" && "$REVISON" > 1 ){
            echo " ROLLBACK"
            sh "helm rollback ${projectName} "
        }else if ("$STATUS" ==~ "(pending).*"){
            echo "Destroy "
            sh "helm uninstall  ${projectName}"
        }
*
