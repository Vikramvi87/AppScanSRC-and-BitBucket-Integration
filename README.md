# AppScan Source and BitBucket Integration
</br>
It will help to Integrate AppScan Source on BitBucket. It will enable the CI/CD to start scan, generate report, publish results to AppScan Source Database and AppScan Enterprise and check for Security Gate.<br>
<br>
Requirements:<br>
1 - AppScan Source in Windows Server.<br>
2 - Add AppScan Source bin folder to Windows PATH Environment Variable.<br>
3 - Install BitBucket Runner for Windows in same Windows Server that has AppScan Source.
4 - Create AppScan Enterprise token <install_dir>\bin\ounceautod.exe -u username -p password --persist.<br>
  Source: https://help.hcltechsw.com/appscan/Source/10.4.0/topics/ounce_auto_login.html <br>
  <br>
```yaml
