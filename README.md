# advance-port-scanner
1.  Here users can specify the ports the number of ports and the host. We will run the programm with the threading library which allows us to scan the ports faster. So we will use different threads to scan the different ports.

2. The 3 libraries that we need to import is the socket library using * Because we want to import everything. The second library will be the optparse, which will allow us to actually specify the help options that will get prompted to the user, such as specifying the ports and hosts. And the third library is the threading library again , we will use * because we want to import everything.

3.  We will have the main function, which will take no arguments ofcourse. Here we will create the help option and call the available options for the user. If the options are well specified by the user then we will continue to actually scan the target and check whether the port is open or closed.

def main():
        parser = optparse.OptionParser('Usage of program: ' + '-H <target host> -p <target ports>')
        parser.add_option('-H', dest='tgtHost', type = 'string', help='specify target host')
        parser.add_option('-p', dest='tgtPort', type = 'string', help='specify target ports sepearated by comma')
        (options, args) = parser.parse_args()
        tgtHost = options.tgtHost
        tgtPort = str(options.tgtPort).split(',')
        if tgtHost == None or tgtPort[0] == None:
                print(parser.usage)
                exit(0)
        portScan(tgtHost,tgtPort)

if __name__ == '__main__':
        main()

4.  The command:
"parser = optparse.OptionParser('Usage of program: ' + '-H <target host> -p <target ports>')"
This is the simple usage of program that will get printed out once th user actually specifies the command wrong, or doesn't really know how the program works.
Specify the -H to the target host which is the IP address of the host we want to scan
and -p to the target port. Now, we have to add both of the options to our parser.  We will use the 'if' function to check whether the user specified the correct arguments for the program. 

5.  Then, let us create the port scan function. 
 What if the user specifies a domain name instead of the IP address? We want to resolve the domain name and get the IP address of the target. For example, if a user type -H for the host and after that types google.com, if our program did not resolve google.com into an IP address, it will crash and it won't be able to scan as the program has no idea what google.com is! To solve this we will use our function 'gethostbyname' in the variable 'tgtIP'. But still there are chances it can crash so let's use 'try and except' rule.

 def portScan(tgtHost, tgtPorts):
        try:
                tgtIP = gethostbyname(tgtHost)
        except:
                print('Unknown Host %s' %tgtHost)
        try:
                tgtName = gethostbyaddr(tgtIP)
                print('scan results for: ' + tgtName[0])
        except:
                print('scan results for: ' +tgtIP)
        setdefaulttimeout(1)
        for tgtPort in tgtPorts:
                t = Thread(target=connScan, args=(tgtHost, int(tgtPort)))               
                t.start()

6.  We have managed to make a function called 'portScan' which will actually try to resolve the IP address by host name and host name by IP address. Then for each port in the specified ports in our command we will run a function called connection scan which will determine whether the port is closed or open. We set that on all of the seperate Threads and we also set the default timer to be one second.

7. Making a function 'connScan' with arguments 'tgtHost' and 'tgtPort'. Now we try to perform the same thing which we did in the previous programs which is trying to connect. Run the try and except rule, make the socket in the try rule.

def connScan(tgtHost, tgtPort):
        try:
                sock = socket(AF_INET, SOCK_STREAM)
                sock.connect((tgtHost, tgtPort))
                print ('%d/tcp Open' % tgtPort)
        except:
                print ('%d/tcp Closed' %tgtPort)
        finally:
                sock.close()

8.  This program is now completed and can now scan any windows machine or website.

9. RESULTS:
<img width="353" alt="Screenshot 2024-02-27 095004" src="https://github.com/juikalan21/advance-port-scanner/assets/159107774/4352efee-e40f-485f-8542-b21510d2e1d1">
<img width="368" alt="Screenshot 2024-02-27 095952" src="https://github.com/juikalan21/advance-port-scanner/assets/159107774/b3e4514f-bd92-4f91-b5c4-4f247d91ace8">

10. Cross checking the adress of the website
<img width="379" alt="Screenshot 2024-02-27 100121" src="https://github.com/juikalan21/advance-port-scanner/assets/159107774/c8d71bca-c4b5-4cd4-9bb0-3f8583ba9bfe">




