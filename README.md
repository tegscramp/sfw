# SFW
Powershell scripts for crypto wallets

# Automatic email reporting sript
for ($i = 1; $i -gt 0; $i++) {
   	$coin = WALLET_PATH\WALLET-cli.exe -rpcuser=USER_NAME -rpcpassword=USER_PASSWORD -rpcport=USER_PORT getbalance
	$date = get-date
	$result = "<div>" + "COIN_NAME: " + $date + " - " + $coin + "</div>"
	$report_subject = "COIN_NAME: " + $date
	$result | Out-File WALLET_PATH\report.txt -Append
	$report = Get-Content WALLET_PATH\report.txt

    #send email
    $From = "examle@examle.com"
    $To = "examle@examle.com"
    $SMTPServer = "YOU_SMTP"
    $SMTPPort = "YOU_PORT"
    $Username = "YOU_LOGIN"
    $Password = "YOU_PASSWORD"
    $subject = $report_subject 
    $body = $report
    $message = New-Object System.Net.Mail.MailMessage $From, $To
    $message.Subject = $subject
    $message.IsBodyHTML = $true
    $message.Body = $body

    $smtp = New-Object System.Net.Mail.SmtpClient($SMTPServer, $SMTPPort)
    $smtp.EnableSSL = $true
    $smtp.Credentials = New-Object System.Net.NetworkCredential($Username, $Password)
    $smtp.Send($message)

	Start-Sleep -s REPEAT_IN_SECONDS
}
