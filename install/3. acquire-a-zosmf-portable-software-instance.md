# Acquire a z/OSMF Portable Software Instance

As a systems programmer, you can acquire an IBM z/OSMF portable package for your product from Broadcom Support and then add the portable software instance to z/OSMF. The product SMP/E environments are pre-built at Broadcom, backed up, and made available for download as a z/OSMF portable software instance. After you acquire the portable software instance, you can use z/OSMF Deployments to perform the installation and z/OSMF workflows to perform post-install configuration.

The following diagram illustrates the acquisition process:

![This image shows the workflow for a systems programmer to order, download, and acquire a portable software instance into z/OSMF.](https://github.com/zeibura/zowe-portable-package-install/blob/main/acquire-package.png)

When you complete the acquisition process, the product software is ready for installation using z/OSMF Deployments.

- **Note**: Before you begin the acquisition process, ensure that you address the z/OSMF requirements.

The z/OSMF product acquisition process consists of the following tasks.

1. Order the z/OSMF portable software instance.
2. Register the portable software instance in z/OSMF.

Refer to the sections below for instructions.

- **Note**: For a list of Broadcom mainframe products that provide a z/OSMF package, see [Mainframe Products using z/OSMF for Software Management](https://techdocs.broadcom.com/us/en/ca-mainframe-software/traditional-management/mainframe-common-maintenance-procedures/1-0/getting-started/z-osmf-requirements/mainframe-products-using-z-osmf-for-software-management.html).

## Order the z/OSMF Portable Software Instance

You can order the z/OSMF portable software instance from the Broadcom Support and then download the package to a local z/OSMF host as a single pax file. You can download directly to the mainframe or you can download to your workstation and then transfer the pax file to the mainframe. A file transfer utility, such as FTP, is required to transfer data to the mainframe.

The portable software instance is a portable form of a software instance, including the SMP/E CSI data sets, all associated SMP/E-managed target and distribution libraries, non-SMP/E-managed data sets, and meta-data that is required to describe the product software instance.

1. Go to Broadcom Support at https://support.broadcom.com and select Mainframe Software.
2. Select Product Downloads and then login to display the Download Management page.
3. Enter the product name or select the product from the list.  
    - Your product entry opens on the Product Download tab.
    - **Note**: To optimize product and solution downloads from Broadcom Support, configure the following URLs in your network security software and/or firewalls:
        - For FTP, ftp://downloads.broadcom.com.
        - For HTTPS, https://downloads.broadcom.com.  
      Sites that regulate access through an IP address must allow network access to 141.202.253.110.
4. Select the hypertext link for the product you want and select a release, service level, and language if applicable.  
The product-specific packages are displayed on the Primary Downloads tab. The Additional Downloads tab, if available, contains common services, utilities, and other files that are associated with the product that you are downloading. These packages may already be installed.
5. Review the packages that are provided under Primary Downloads and Additional Downloads and select the zOSMF Package (*productid*.V*nn*R*n*.ZOSMF.pax.Z) and other files that you want to include in the product download. Use the up and down arrows to expand and collapse the package contents as necessary. If more than one package is available, you can only select one.
    - **Warning**: To ensure that you have the files that you want, ***do not*** select **Add All to Cart** or **Download Package**.  
Select the **Add to Cart** option, **HTTPS**, or **FTP**. FTP is the preferred download method. For download tips, see [Download & Search Solution Help](https://knowledge.broadcom.com/external/article/142936).
6. Execute JCL to unpack the installation file and restore the individual pax files. Sample JCL follows:  
` //USSBATCH EXEC PGM=BPXBATCH,`  
`//STDOUT   DD=SYSOUT=*`  
`//STDERR   DD=SYSOUT=*`  
`//STDPARM  DD=SYSOUT=*`  
`sh cd /yourUSSpaxdirectory/;`  
`pax -rvf yourpaxfilename.ZOSMF.pax.Z`  
`/*`  
Customize the sample JCL as follows and then submit for execution:
    1. Add a JOB statement.
    2. Update the USS directory (*yourUSSpaxdirectory*) with the path name where you want to copy the pax file.
    3. Update *yourpaxfilename* with the name of the pax file that you want to copy to the mainframe.  
USSBATCH can take several minutes to execute. A return code of zero is expected. Any other return code indicates a problem.  

After successful execution, the individual pax files have been restored and are ready for use. Go to Register Portable Software Instance in z/OSMF.

## Register Portable Software Instance in z/OSMF

After you have acquired and downloaded the portable software instance to a local z/OSMF host system, you must log in to z/OSMF to register the product software and define the portable software instance to z/OSMF as shown in the following procedure. When you complete these steps, the portable software instance is registered in z/OSMF and ready for installation (deployment).

1. Log in to the z/OSMF web interface and select your user ID in the top or bottom right-hand corner to switch between the Desktop Interface and Classic Interface.
2. Complete *either* of the following steps to display the Software Management page:
    1. In the Desktop Interface, select **Software Management**.
    2. In the Classic Interface, select **Software**, **Software Management**.
3. Select **Portable Software Instances** to define your portable software instance to z/OSMF.
4. Select **Add** from the Actions menu and select **From z/OSMF System**.  
The Add Portable Software Instance page displays.
5. Select or type the system name (destination LPAR) and UNIX directory (destination USS directory) where the portable software instance files reside and select **Retrieve**.
6. Enter a name for the new portable software instance. You can also enter an optional description and assign one or more categories that display existing packages.
7. Select **OK**.  
The new portable software instance is now defined to z/OSMF.

The portable software instance is now registered in z/OSMF and ready to install (deploy).
