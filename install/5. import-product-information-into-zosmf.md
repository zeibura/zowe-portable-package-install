# Import Product Information into z/OSMF

As a systems programmer, your responsibilities include maintaining a current repository of acquired product packages that are ready for installation in your mainframe environment. z/OSMF lets you view a consolidated list of the products and maintenance packages that are included in each software instance or portable software instance. You can import product information for the Broadcom mainframe products that you have installed on z/OS into z/OSMF. After retrieving the information, you can use the Products page in the z/OSMF Software Management task to obtain a list of products that are contained in your software instances. This information helps you to determine which products are nearing or have reached end of service (EOS) or end of life (EOL) support. This information is useful when planning for future installations and upgrades. You can also use this information to identify software instances that will be affected by changes to a product. The product information file for Broadcom mainframe products is stored on the following FTP directory:

`https://ftpdocs.broadcom.com/WebInterface/phpdocs/0/MSPSaccount/COMPAT/BroadcomProdInfo.txt`

To prevent SSL handshake failures when importing product information into z/OSMF, use the following procedure to enable secure downloads:

1. Download the certificate from the following URL:  
https://ftpdocs.broadcom.com/WebInterface/redirectFTPDocs.html?path=/cadocs/0/certs/digicert-old/Digi-Intermediate.crt  
Note the location on your workstation where the certificate was downloaded.
2. Upload the certificate as text data to your z/OS system in RECFM=VB and LRECL=84 format.  
If you use FTP, use the following FTP commands to avoid truncation:  
`ASCII`  
`QUOTE SITE WRAP LRECL=84 RECFM=VB`  
`PUT your_PC_file_name 'your.mvs.dataset.name' (REPLACE`  
`quit`
3. Add the certificate to the existing z/OSMF IZUSVR keyring.
    - **For ACF2, specify:**  
    `SET PROFILE(USER) DIV(CERTDATA)`  
    `INSERT CERTAUTH.yourcertname DSN('your.zos.dataset.name') -`  
    `LABEL(yourlabeldescription)`  
    `CONNECT CERTDATA(CERTAUTH.yourDigicertCAcertname) KEYRING(IZUSVR.keyr01) RINGNAME(IZUKeyring.IZUDFLT) USAGE(CERTAUTH)`
    - **For Top Secret, specify:**  
    `TSS ADD(CERTAUTH) DIGICERT(yourDigicertCAcertname) LABLCERT(yourlabelname) - `  
    `DCDSN('your.zos.dataset.name') TRUST`  
    `TSS ADD(IZUSVR) KEYRING(zosmfringname) RINGDATA(CERTAUTH,yourDigicertCAcertname) - `  
    `USAGE(CERTAUTH)`
    - **For IBM RACF, specify:**  
    `RACDCERT CERTAUTH ADD('your.zos.dataset.name') WITHLABEL('yourlabelname') TRUST`  
    `RACDCERT ID(IZUSVR) CONNECT(CERTAUTH LABEL('your_digicertCA_label') +`  
    `RING(keyringname) USAGE(CERTAUTH))`

To load the contents of this file into z/OSMF, you can:
- Load the file from the Broadcom URL
- Load the file from your local workstation
- Load the file from a z/OS data set or UNIX file

After the file is loaded, you can then retrieve the product information in z/OSMF.

**Note**: If you create software instances or portable software instances in z/OSMF, import the product information file again so that you have current information that is displayed about your installed products. We recommend that you repeat the following procedures on a regular schedule or at least monthly to ensure that you have current Broadcom product information available in z/OSMF. This process ensures that you have access to all product packages as they become available. For more information, see [Retrieving product information from product information files](https://www.ibm.com/docs/en/zos/2.4.0?topic=information-retrieving-product-from-product-files) in the IBM documentation.

## Load the File from the Broadcom URL

Use the following procedure to load the contents of the Broadcom product information file into z/OSMF directly from the Broadcom FTP directory.

1. Log in to the z/OSMF web interface.  
2. Complete *either* of the following steps:
    - In the **Desktop Interface**, select **Software Management**.
    - In the **Classic Interface**, select **Software**, **Software Management**.  
    The Software Management dashboard displays.
3. Select **Products**.  
The Products table list is displayed.
4. Select **Retrieve End of Service Information** from the Actions menu or select the Retrieve End of Service Information button if available.  
The Select Product Information File page displays.
5. Select the option to **Select files that reside on or can be accessed by primary system** and select **Add** from the Actions drop-down menu under Product Information Files.  
The Add Product Information File page is displayed.
6. Select **URL** and then specify the following URL in the **URL** field.  
`https://ftpdocs.broadcom.com/WebInterface/phpdocs/0/MSPSaccount/COMPAT/BroadcomProdInfo.txt`
7. Specify a description in the **Description** field.
8. Select **OK**.  
The product information file table is updated with the new URL.
9. Select the new URL and select **Retrieve**.  
The contents of the file are loaded into z/OSMF.

You can now view the Broadcom product information that has been retrieved from the Software Management dashboard.

## Load the File from Your Local Workstation

Use the following procedure to load the contents of the Broadcom product information file into z/OSMF from your local workstation.

1. Download the product information file from the following Broadcom FTP directory to your local workstation using your Web Browser or an FTP client:  
`https://ftpdocs.broadcom.com/WebInterface/phpdocs/0/MSPSaccount/COMPAT/BroadcomProdInfo.txt`
2. Log in to the z/OSMF web interface.
3. Complete *either* of the following steps:
    - In the Desktop Interface, select **Software Management**.
    - In the Classic Interface, select **Software**, **Software Management**.  
    The Software Management dashboard displays.
4. Select **Products**.  
The Products table list displays.
5. Select **Retrieve End of Service Information** from the Actions menu.  
The Select Product Information File page displays.
6. Select the option to **Select a file that resides** on your local workstation, enter your file name and description, and select **Retrieve**.  
The contents of the file are loaded into z/OSMF.

You can now view the product information that has been retrieved from the Software Management dashboard.

## Load the File from a z/OS Data Set or UNIX File

Use the following procedure to load the contents of the Broadcom product information file into z/OSMF from a z/OS data set or UNIX file that the primary z/OSMF host can access.

1. Use FTP to download the product information file directly to the mainframe. Upload the file with binary in the FTP JCL so that the file is not converted to the EBCDIC character set.  
Sample JCL follows that you can customize and execute:  
`//FTPSTEP  EXEC PGM=FTP,PARM='(EXIT=08'`  
`//SYSTCPD  DD DSN=your_TCP/IP_data_set_name,DISP=SHR`  
`//SYSPRINT DD SYSOUT=*`     
`//OUTPUT   DD SYSOUT=*`  
`//INPUT    DD *`   
`ftp.broadcom.com 21`   
`anonymous email_address`   
`cd /pub/MSPSaccount/JSON/`   
`dir`   
`asc`   
`locsite LR=80 REC=FB BLOCKSI=0`   
`locsite PRI=20 SEC=10 CY`   
`get BroadcomProdInfo.txt 'z/OS_data_set' (REPLACE`  
    - **your_TCP/IP_data_set_name**  
    Specify the TCP/IP stack for an external network.
    - **email_address**
    Specify your valid email address.
    - **z/OS_data_set**
    Specify the z/OS data set name where you want to save the product information file. If the specified data set does not exist, it is created during the download process.
2. Log in to the z/OSMF web interface.
3. Complete *either* of the following steps:  
    - In the Desktop Interface, select **Software Management**.
    - In the Classic Interface, select **Software**, **Software Management**.  
The Software Management dashboard displays.
4. Select **Products**.  
The Products table list displays.
5. Select **Retrieve End of Service Information** from the Actions menu or select the Retrieve End of Service Information button if available.  
The Select Product Information File page displays.
6. Complete the following fields:
    1. Select the option to **Select files that reside on or can be accessed by primary system** and select **Add** from the Actions drop-down menu under Product Information Files.  
    The Add Product Information File page displays.
    2. Select **Primary z/OSMF system** and specify the *z/OS_data_set_name* in the **Data set** or **UNIX file** field.  
        - **Note**: The primary z/OSMF host system must be able to access the specified data set or UNIX file.
    3. Specify a description in the Description field and select OK.  
    The product information file table is updated with the new URL.
7. Select the new URL and select **Retrieve**.  
The contents of the file are loaded into z/OSMF.

You can now view the product information that has been retrieved from the Software Management dashboard.
