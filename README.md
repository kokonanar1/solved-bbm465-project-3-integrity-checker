Download Link: https://assignmentchef.com/product/solved-bbm465-project-3-integrity-checker
<br>
You are expected to develop a directory integrity checking program. The program monitors the changes on a specified directory using hash functions and digital signatures. The program must keep track of all changes, such as deleting, altering, and creating files in the monitored directory and record all these changes to a log file. The program should have three main functionalities: 1) create public/private key pairs and public key certificate 2) create a registry for a directory 3) check changes on the directory using registry file. The program must be developed as a console application and names as “<em>ichecker”</em>.

<strong>Creating Public/Private Key Pair:  </strong>

Before starting monitoring operations, the program must be run to create public/private keys and associated public key certificate. The program should be run as follows:

<em>ichecker createCert -k PriKey -c PubKeyCertificate </em>

<ul>

 <li><em>-k </em>specifies the path of the private key file, which is encrypted with AES algoritm.</li>

 <li><em>-c </em>specifies the path of the public key certificate file</li>

</ul>

When this command is executed, <em>ichecker</em> will use <em>keytool</em> [1] to create a random public and private key pair for 2048 bit RSA algorithm. It also asks user to enter a password, which is be used to encrypt the private key file. When the user enters a password, MD5 hash operation is applied on the password and 128 bit output of MD5 function is be used as an AES key to encrypt private key file, which is specified with <em>–k</em> parameter. The private key file must contain some additional meaningful text data (such as, “This is the private key file”) to understand if the user enters wrong password in the later operations.

Additionally, using <em>keytool,</em> the program will create a X.509 certificate file, which is specified with <em>–c</em> parameter. The certificate file contains the public key and must be signed with the private key (public and private key are the keys created by <em>keytool</em>). In other words, you need to create a self-signed certificate. This certificate and encrypted private key file will be used in the later operations.




<strong>Creating Registry File:  </strong>

To start monitoring a directory, the program must be run to create a registry for the monitored directory. The program should be run as follows:

<em>ichecker createReg -r RegFile -p Path -l LogFile -h Hash -k PriKey </em>

<ul>

 <li><em>-r </em>specifies the path of the registry file.</li>

 <li><em>-p </em>specifies the path of the directory to be monitored by the program.</li>

 <li><em>-l </em>specifies the path of the log file.</li>

 <li><em>-h </em>specifies a hash function, which can be “MD5” or “SHA-256”.</li>

 <li><em>-k </em>specifies the path of the private key file, which is encrypted with AES algoritm.</li>

</ul>

After running above command, the program asks user for a password. When the user enters a password, AES key is obtained by applying MD5 hash operation on the password. Then, the program uses AES key to decrypt the private key file, which is specified by <em>–k</em> parameter. If the user enter a wrong password, the program must understand it by checking decrypted content of the private key file and terminate itself. This wrong password attempt must be logged to the log file, which is specified by <em>–l</em> parameter. The log format should be like this:

<em>[time_stamp]: Wrong password attempt! </em>

The format of the <em>time_stamp</em> should be in <em>time stamp = dd-MM-yyyy HH:mm:ss </em>format. Then the program creates the registry file specified by <em>–r </em>parameter. The registry file stores the path of all files in the monitored directory specified by <em>-p </em>parameter and hash values of file contents, which is calculated by using the hash algorithm specified by <em>-h </em>parameter. The format of registry file is as follows:




<em>[Path_of_file1] H(file1)             </em>

<em>[Path_of_file2] H(file2)       [Path_of_file3] H(file3)      </em>

<em>. . .                                 </em>

<em>#signature#                          </em>

<em> </em>

<em> </em>

In this registry file, <em>[Path_of_file] </em>refers the full path of a file and <em>H(file)</em> refers to hash digest of the file. The last line of the registry file contains a signature, which is obtained by calculating the hash value of all content of the registry file except the last line and then by signing the hash value with the private key specified by <em>-k </em>parameter.




The program must also record the whole operation to the log file, specified with <em>– l</em> parameter. A sample for the log file after executing the above command should be like this:




<em>[time_stamp]: Registry file is created at [Path_of_RegFile]!  </em>

<em>[time_stamp]: [Path_of_file1] is added to registry. </em>

<em>[time_stamp]: [Path_of_file2] is added to registry. </em>

<em>[time_stamp]: [Path_of_file3] is added to registry. </em>

<em>. . . </em>

<em> </em>

<em>[time_stamp]: 16 files are added to the registry and registry creation is finished!  </em>




<strong>Checking Integrity:  </strong>

After creating a registry file, the integrity of directory can be checked by using <em>ichecker. </em>To do that, the program should be run as follows:




<em>ichecker check -r RegFile -p Path -l LogFile -h Hash -c PubKeyCertificate </em>




In this command, <em>-c </em>specifies the path of the public key certificate file. When this program is executed, the registry file must be verified first by checking the signature located at the end of the file. For signature verification, the public key in the certificate must be used. If the verification process fails, it must be recorded to the log file specified by <em>-l </em>parameter as follows:




<em>[time_stamp]: Registry file verification failed!  </em>




Then, the program must terminate. If the registry file verification is successful, the changes in the directory is checked. If there is no change in the directory, the following log is written to the log file:




<em>[time_stamp]: The directory is checked and no change is detected! </em>




If there are some changes in the directory, the changes are recorded to the log file as follows:




<em>[time_stamp]: [Path_of_file] is [change_type] </em>




In this log line, the <em>change_type</em> parameter can be one of the following values:




<em>change_type = deleted | altered | created </em>




<strong>           </strong>

<strong> </strong>

<h1>1               Notes</h1>

<ol>

 <li>You can ask questions about the experiment via Piazza group (piazza.com/hacettepe.edu.tr/fall2020/bbm465).</li>

 <li>Late submission will not be accepted!</li>

 <li>You must compile your program on Java version of the dev machine.</li>

 <li>You are going to submit your experiment to online submission system:</li>

</ol>

<a href="http://submit.cs.hacettepe.edu.tr/">http://submit.cs.hacettepe.edu.tr/</a>




<ol start="5">

 <li>The submission format is given below:</li>

</ol>




<em>&lt;</em>Group id<em>&gt;</em>.zip

<em>−</em>[src]/

<em>−−</em>*.[java]


