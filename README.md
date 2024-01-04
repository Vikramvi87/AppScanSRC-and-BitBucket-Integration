# AppScan Source and BitBucket Integration
</br>
It will help to Integrate AppScan Source on BitBucket. It will enable the CI/CD to start scan, generate report, publish results to AppScan Source Database and AppScan Enterprise and check for Security Gate.<br>
<br>
Requirements:<br>
1 - AppScan Source in Windows Server.<br>
2 - Add AppScan Source bin folder to Windows PATH Environment Variable.<br>
3 - Install BitBucket Runner for Windows in same Windows Server that has AppScan Source.<br>
4 - Create AppScan Enterprise token <install_dir>\bin\ounceautod.exe -u username -p password --persist.<br>
  Source: https://help.hcltechsw.com/appscan/Source/10.4.0/topics/ounce_auto_login.html <br>
  <br>

```yaml
pipelines:
  default:
    - step:
        name: 'SAST Scan'
        runs-on:
          - self.hosted
          - windows
          - appscan
             
        script:
          - $aseToken='C:\ProgramData\HCL\AppScanSource\config\ounceautod.token'
          - $aseHostname='xxxxxxxxxxxxx'
          - $aseApiKeyId='xxxxxxxxxxxxx'
          - $aseApiKeySecret='xxxxxxxxxxxxx'
          - $aseAppName='AltoroJ'
          - $compiledArtifactFolder='none'
          - $scanConfig='Normal scan'
          - $sevSecGw='highIssues'
          - $maxIssuesAllowed='1000'
          - $buildNumber=$env:BITBUCKET_BUILD_NUMBER
          
          - Invoke-WebRequest -Uri https://raw.githubusercontent.com/jrocia/AppScanSRC-and-BitBucket-Integration/main/scripts/appscanase_create_application_ase.ps1 -OutFile appscanase_create_application_ase.ps1
          - .\appscanase_create_application_ase.ps1         
          
          - Invoke-WebRequest -Uri https://raw.githubusercontent.com/jrocia/AppScanSRC-and-BitBucket-Integration/main/scripts/appscansrc_create_config_scan_folder.ps1 -OutFile appscansrc_create_config_scan_folder.ps1
          - .\appscansrc_create_config_scan_folder.ps1
          
          - Invoke-WebRequest -Uri https://raw.githubusercontent.com/jrocia/AppScanSRC-and-BitBucket-Integration/main/scripts/appscansrc_scan.ps1 -OutFile appscansrc_scan.ps1
          - .\appscansrc_scan.ps1
          
          - Invoke-WebRequest -Uri https://raw.githubusercontent.com/jrocia/AppScanSRC-and-BitBucket-Integration/main/scripts/appscansrc_publish_assessment_to_enterprise.ps1 -OutFile appscansrc_publish_assessment_to_enterprise.ps1
          - .\appscansrc_publish_assessment_to_enterprise.ps1
         
          - Invoke-WebRequest -Uri https://raw.githubusercontent.com/jrocia/AppScanSRC-and-BitBucket-Integration/main/scripts/appscansrc_check_security_gate.ps1 -OutFile appscansrc_check_security_gate.ps1  
          - .\appscansrc_check_security_gate.ps1
          
          - mkdir reports
          - Move-Item -Path *.ozasmt -Destination .\reports
          - Move-Item -Path *.pdf -Destination .\reports
          
        artifacts:
          - reports\*.*
```
