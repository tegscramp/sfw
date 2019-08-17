# Scripts for wallets
Powershell scripts for crypto wallets.
Powershell скрипты для атоматизации рутинных задач с криптокошельками.

## Automatic email reporting sript
Скрипт для отправки отчета по балансу через определенный интервал времени.
Алгоритм формирования отчета следующий: 
- Создается файл report.txt;
- Получаем текущий баланс кошелька и записываем его в конец файла report.txt;
- Данные из report.txt отправляются на указанную электронную почту.
- Действие зациклено и повторяется через указанный интервал времени.

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
