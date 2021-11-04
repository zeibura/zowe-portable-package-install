# Address z/OSMF Requirements

Before you install your Broadcom mainframe products using IBM z/OSMF, address the following installation and security requirements. Your systems programmers and security administrators can complete these tasks in parallel.

* **Apply required maintenance for Common Components and Services for z/OS (CCS) Version 15.0 (SO12499)**  
    * **Role**: Systems programmer
    * The CCS PTF installs load module stubs for select IBM products into your installed CCS library hlq.CAW0CALL. If you are prompted during installation for the data set name of a load library for an IBM product that is not installed, specify your installed hlq.CAW0CALL data set name.  

    
