## SQL SERVER 2019 Always On High Availability Setup
![](https://lh7-us.googleusercontent.com/-yEZVUCZ8ERfxCts23BqMFpnZ50ogl31NuK3aILDlDW3yjtmLwbcL6eVabBmChC4hhNYltpPxRtXMfjKF5QpWwKtlIoqje5wu4qk_RS1CXCRcUhu09nuabnsv3VA1GzN9KU9dIvXLAksxcI5i7YfCo0)

***
Microsoft SQL Server High Availability (HA) Setup Outline:
1. **VMware Workstation Configuration:**
- Configure VMware Workstation for virtualization.
2. **Create Virtual Machines:**
- Create three virtual machines: DC-1 (Domain Controller), SQLNode1, SQLNode2.
3. **Network Configuration:**
- Configure network settings in VMware:
- Set up VMnet3 or preferred network adapter.
- Assign IP addresses:
- DC-1: 192.168.254.100
- SQLNode1: 192.168.254.2
- SQLNode2: 192.168.254.3
4. **Changing Computer Names:**
- Rename computers:
- Domain Controller: DC-1
- Node1: SQLNode1
- Node2: SQLNode2
5. **Active Directory (AD) Setup:**
- Install Active Directory Domain Services (AD DS) on DC-1:
- Configure the domain as "homelab.com."
- Create necessary Organizational Units (OUs), user accounts, and groups.
6. **Domain Controller Configuration:**
- Set up DNS and other necessary services on DC-1.
- Promote DC-1 as the domain controller.
7. **Installing Failover Cluster Feature:**
- Install Failover Cluster feature on SQLNode1 and SQLNode2:
- Configure shared storage if required.
- Validate the cluster setup.
- Configure the Cluster Name and IP: SQL_WFCS , 192.168.254.4
8. **SQL Server Installation and Configuration:**
- Install SQL Server on SQLNode1 and SQLNode2:
- Configure a failover cluster instance.
- Set up default instances, service accounts, and mixed mode authentication.
9. **Creating Availability Group (AG):**
- Create an AG between SQLNode1 and SQLNode2:
- Set automatic failover, backup preferences, and shared backup directories.
10. **SQL Listener Configuration:**
- Configure SQL listener (e.g., sql-listener) name and IP (e.g., 192.168.254.5) in DNS manager.
- Create a SQL listener object in AD (under SQL_WFCS) and grant appropriate permissions.
- Add SQL listener to the availability group.
11. **Windows Firewall and Security Configuration:**
- Configure Windows Firewall settings as needed.
- Implement security measures and access controls.
12. **Testing and Verification:**
- Validate the SQL Server HA setup:
- Test failover scenarios.
***
## Step By Step Guideline
**Configure Network**
-   Access Virtual Network Editor in VMware.
-   Set up a new or use an existing network adapter (e.g., VMnet3) for the virtual machines.
![](https://lh7-us.googleusercontent.com/aHoDM9LZFIoxwg2hm627ATfhs-IMsDFxsB7qH0xec32Rc-2c3eyjj7PyZSuq70tZeRqovZnEExLaTxoy0Wc716r3kb6oLGWfZuIuYYII_XmoJRpuacV9FNtNYFwoRXNA-dCLmWkDYYV8F9v5tq83mQA)

**Changing Computer Name and Assigning IP:**
***Changing Computer Name:***
-   On each Windows Server:
-   Right-click on "This PC" or "My Computer."
-   Select "Properties."
-   Click on "Change settings" next to the computer name.    
-   Click on "Change" to modify the computer name.    
-   Enter the new computer name and click "OK."    
-   Restart the system for the changes to take effect.  

***Assigning IP Address:***

-   DC-1 Configuration:    
-   Go to "Network and Sharing Center."    
-   Click on "Change adapter settings."  
-   Right-click on the network adapter and select "Properties."    
-   Select "Internet Protocol Version 4 (TCP/IPv4)" and click "Properties."    
-   Choose "Use the following IP address."   
-   Enter the following:    
-   IP Address: 192.168.254.100    
-   Subnet Mask: 255.255.255.0    
-   Default Gateway: 192.168.254.1    
-   Click "OK" to save the settings.  
![](https://lh7-us.googleusercontent.com/US7NIMGOZGg2fSTmA2rvi4Z3jgXvNiYOy8DDUBIgEEO4c2eC_1i1_F6DM6gFSPr5PjRBujiNQoIt3HAJYopoNqrHpWeQF3fin1N1uCljegdBd_BM0-7doYdjRXZJ0rKacnRX8bHBJN3JrvQDZBjPBWg)
![](https://lh7-us.googleusercontent.com/jh43aPvDrzSs_bJPZv_-0v-QkOF6XQLTRGlXuUZukBoZNqRxEEs4g5S0Q327D3OJ8YUs9TAKKc756udINLiH9TCFLrLIuo7m_da1HRymiqifoPEcAKLxBM5__jZ5smRcwWWI--pmf60o_LwiC1AoW2Y)
![](https://lh7-us.googleusercontent.com/CYgKGtYPV4ObLuehmecU7AvecrU8JkDKkmvZLz065rMlzhJlsGBiTj_CxoJiALyH0Ag6MbFVWmTKGrNJhGb0w87vRFrnEryeIwdB6vpvcr1oKkguGsIke1mxsbTQAucyOo5WLda6G9uFeSEHX0-SI_8)
**Follow the same steps for node1 and node2 as well**
**SQLNode1 Configuration:**    
Use the following IP settings:    
-   IP Address: 192.168.254.2    
-   Subnet Mask: 255.255.255.0    
-   Default Gateway: 192.168.254.1
***
![](https://lh7-us.googleusercontent.com/r56FtDpCW1VxCF3-wLLCrhWhIgKQlb7Db3924QgC4ET5OIdzGFgh6ju2yuPi66jbcCjtb5TN2FUo_-ypf2wYj3oIn-TV8H9X-RlVK92bJ9q-0_H02syAwsgI2_dJUFiAx5lXM6MqgEKAgIJODVfQgWQ)
**SQLNode2 Configuration:**
Use the following IP settings:     
-   IP Address: 192.168.254.3    
-   Subnet Mask: 255.255.255.0    
-   Default Gateway: 192.168.254.1
***
![](https://lh7-us.googleusercontent.com/6DcpAsefJHUdN7qiGxeqph81laHFsivaBPD4DBUzTyg1am9ZH0LJTVRTrzmpqiT36EvkZHL8pg3osvOVvKV5eJ1yKer7-aR8lXUEbQeQDEqRAy7oOr8hRMb1RFIKRBdUQs3iTU47MVt8tj_suXlarsw)
***
**Install .NET Framework 3.5 Features (Optional)**
-   Access Server Manager:    
-   Open "Server Manager."    
-   Initiate Feature Installation:    
-   Navigate to "Manage" > "Add Roles and Features."    
-   Installation Process:    
-   Select "Role-based or feature-based installation" and proceed.    
-   Choose the local server and move forward.    
-   Select .NET Framework 3.5:    
-   Locate ".NET Framework 3.5 Features."    
-   Check the box for ".NET Framework 3.5 (includes .NET 2.0 and 3.0)."    
-   Additional Features (if prompted):    
-   If required, add additional features and proceed.    
-   Confirmation and Installation:    
-   Review selected features and initiate the installation.    
-   Wait for the process to complete.    
-   Completion:    
-   Once installed, a success message will display.    
-   Close the wizard to finish.     
![](https://lh7-us.googleusercontent.com/XiY9QekyjIO7qDJEKbuG5pGrW2EOsjSZDWYI9CEQ1BVvRuwZtRFXjQidoXNameZ53zktHk0fmoCpFA75sgL38KcMhB-hjZItS8LoPVBxcqqrknFI34tokXX87h_GbaiIzLnHnqtud9f4RbA2dNgq1y0)
![](https://lh7-us.googleusercontent.com/4vxuEFsO5KfhUipuZ903G-4vilppPdDl5RwUuwk0FxiVjJt9_FaaPTRuwM7KJS5WNM_SdzcrnRQHZCIpRKmeBfBydH_a9tG_PHAAoEgR5moLbeGP9Uu9xZyFtVosuVgb2HnVcj-sooaEIlzVZrdJ51g)
![](https://lh7-us.googleusercontent.com/EKAR_1pYmWOe7EKpTUGZ6Tu3CDWNP2e9bJ6lisoU5b2JOEZ7UQUhypyiyoY85_uB1oDXkk1A9p4gNncUIDUzGKdhrEJ_CEBp3qp-cUAZBpf94F6I7ApqLytl8-5ariCi_PGLOXno00a059xWMjo9pZw)
![](https://lh7-us.googleusercontent.com/D3tuJQS0cloOvmUsDDn80bZF11bthQ9m3vPmk4RFI0PDohIP3RmF0opYsL-jX_kJaO-Z_ISw8slSM5McFoMmh6EL79RIITYs1reSNUEx6pwpadQPAs3eqwUL-pAkFWdfhutVD6Istd4Lkb6hF6MbYmg)
***
 **Steps for Setting Up AD DS, DNS Server, and DC:**
 ***Active Directory Domain Services Installation:***    
-   Open 'Server Manager' on the designated server (DC-1).    
-   Click on 'Add roles and features' and select 'Active Directory Domain Services' to install the AD DS role.    
-   Complete the AD DS installation process.
***
![](https://lh7-us.googleusercontent.com/GmOzrBti1s66Jd-xdSnfqO3nFNYnliNXWZmXrr3lsQVfgfqDuU-pfTW6WasQmEbo9fnlf5jlOzH-3LkFv4YsNh9wHi1MHTB9xQn0jmosOhkjyTWcSmGTJUZPUW4DMJ3CpBGo4jKp0jZorrUgcuz_JAo)
![](https://lh7-us.googleusercontent.com/TTn4RvG4MQszc8RVYmtzgdoSPAyr38pz_L87N5sUsZodxRF4TwsQr_4lqe2nQdq3asAihZ28BEYJFb25G1rRm8JwIe1kZUGyNK6I9KTucivLpHfbVnfIWYOzDdNM8LERh_jAHA0EtimucBAeDMf5qMs)
***
***DNS Server Tool Installation:***   
-   After installing AD DS:    
-   Open 'Server Manager'.    
-   Click on 'Add roles and features' and select 'DNS Server' to install the DNS role.    
-   Complete the DNS Server installation process.    
***
![](https://lh7-us.googleusercontent.com/JS7GvcJzhSV8ay7obRPyGQ0OCqel0YKORcddztiDa7l24ggoZZpyA4FPfGeEE26ky6_iIf5fTvD7uhp7_VQiCb1DORb5v2sH9RziBVy6W8gDx_dsu1qkBbpdMATweHpB6OTLY0mRyOLrwzf5qdm3Q94)
![](https://lh7-us.googleusercontent.com/FWG2iZMvGjLxq60f3-y333RuCr-ySfzOXXGgaHF863MrlT6s-kg_Q04np-jFRUh8r0xEfyTkxtJNjRKhcP6T6_J6DNnDiR5uJJdZbgnA9hRQ7AEEoIQdJUPvL7pg-2F0ZGyPwjLzxxQxDpTpmvIES_U)
***
***Promoting to Domain Controller:***    
-   After installing both AD DS and DNS Server roles:    
-   Open 'Server Manager'.    
-   Click on 'Promote this server to a domain controller'.    
-   Choose the option 'Add a new forest' since it's the first domain in your environment.    
-   Enter the root domain name (e.g., homelab.com).   
-   Set the Directory Services Restore Mode (DSRM) password.    
-   Choose the forest and domain functional levels.    
-   Review configuration summary and click 'Install'. The server will reboot after completion.
***    
![](https://lh7-us.googleusercontent.com/GrR_d1TWrvcWD49JsExfxEbm6-hK20byZMfGfKO8KFSUCcMNmfjqeQdLT7lhlAPZE6Mfmv9mhGuzbF0LobXVVYhVVE5tH-E906vFEFDRaqQeGE8YPH7RZO73BKMTL7gAY8O3rvCFirNf3mrVzkztgH8)  
![](https://lh7-us.googleusercontent.com/6WT30_o_0VHjhLM2GCRUAogHuC7Y0zWpxD4YhOZRG_-HurpVEv417bjxTxwJkIR6UCMMT11vzwamDkHa4Ex0nCpZ8l9CPWQ97j5sW-pKoggwmDaoEHggWR_SDQK9BR7lFWS3z8ZUFUZ97aH4nWovAkY)
![](https://lh7-us.googleusercontent.com/-6URPBfGuGTy5MQPI9nkGwphpT-R9MU-gDhEwjU4fBUj_B5ctGqgZzH1OhlhFRHnLmHYmHyuwI5fs5Rx0ZS63v5XCxbe_IK2QgVjLz7rqI05bsBsYQ-lky8RgtT7-gqi4Nzcpv3ssRCaOksIP0Y6yRY)
![](https://lh7-us.googleusercontent.com/_0boLOX3Z8P80cPXFiSEZiHoMbMtP7QRgZnIJwP_gPJh40wYssHwqwdAuJL6OHc090VuW26BR9bedF9Egu-n212JvH27Bz4LNp10rn2UwHBMLA90frXx1547m_-6uJ1jawlsOWzrTvTteaQ-STx1a1k)
![](https://lh7-us.googleusercontent.com/ORHB8N-SQhJPFXHVz5ULzooF4UAXmjl0UcmZ2l7jBf1xC5ufvkDIq9D0VmWLKYIcCJqjZjZ6eI8oNpMg02jYwBoebO5uOLZP6RsHa6UjhMlx9kZbLQoQ5gAl_RogIKCLI3cn5FSZl9Ne7ZFEuIyubVw)
***
**Active directory users and computers**
***Steps to Create Organizational Units (OUs):***
-   Open Active Directory Users and Computers:    
-   Log in to the Domain Controller (DC-1).    
-   Open 'Server Manager'.    
-   Go to 'Tools' and select 'Active Directory Users and Computers'.
***
![](https://lh7-us.googleusercontent.com/tHelmVEFbR_7njgaG1IuBwjeDpGwQxTZBXTL_NoHYVD1aCDcyC5MHLVUCUpjDb77I6OeZtul1r6Sc98ZljmMjkCh2gdQ4Hmxq8rmocjsiKgNO0kSLSx4huOl5KUoAEDt0hBpyAfwo1oq6zNhPMaLkYo)
-   Create "SQL Servers" OU:    
-   Right-click on your domain name (homelab.com) or an existing OU.    
-   Choose 'New' > 'Organizational Unit'.    
-   Name the new OU as "SQL Servers" and click 'OK'.    
-   The "SQL Servers" OU is now created within Active Directory.    
***
![](https://lh7-us.googleusercontent.com/ugQ0rLBs37N2M1QJv1JuroefMRfrw_UXP20bvKJL24Vu0jRZCB72SGQRaJSAWRRJ5XWNzO-jG8CD_6Cu6DUQIvIBMUsD1oSzG89JwgZCOHZN6aFSWXq9cBjHJFAiSia0-7K4Nz5iyOyqoSDX26LioUE)
![](https://lh7-us.googleusercontent.com/gDmRwxnuCOAvNPDv7aG__hQ14y8nhzJAGdRaaYKSa06e04C_btnJ6P8NJTh3Qz0byvx0uX9nIwtnv7_nqg7l8UkXfjwheHKDDDu6djRkYfpatUlR8-7rB8IPzeJ55tx6VnH-QQG-XmWL3doDujPQG8s)
-   Create "Admin Users" OU:    
-   Similarly, right-click on your domain name (homelab.com) or an existing OU.   
-   Select 'New' > 'Organizational Unit'.    
-   Name the new OU as "Admin Users" and click 'OK'.    
-   The "Admin Users" OU is now created within Active Directory.    
![](https://lh7-us.googleusercontent.com/a6UpkirOMzWTbs7pasBem93f__sfPgQVqCLDOO25oGKNbNlM84odRoJxSVw_n8gArr-SI3dZKbNWHJoyDYe1ouM5o65OLtsqYEbL6b6mhtYjG0XFt9UvhpMB4wMLu1PVJOE4yzLBgZu0dEnDzo5pxv8)  
![](https://lh7-us.googleusercontent.com/yuRQrnlupplU-Mlc0InhurYXgsqQE1-f5KrJJ7Zmiaasy4EUl842YBqTqgOFtDoP28UqUQmO3Xdf7d_0P_dbhDnMflbmJpo8hs1C5R1Is4Rzj7jyz2zgyibJqQJOW_v9DLeslwCyCcSskvT_PmNNoCw)
***
**New user inside admin for SQL**
***Steps to Create a User and Add to Domain Admins Group:***
-   Create "sql admin" User:    
-   Open 'Active Directory Users and Computers'.   
-   Navigate to the "Admin Users" OU.    
-   Right-click on "Admin Users" and choose 'New' > 'User'.    
-   Fill in the required user details:    
-   First name: SQL    
-   Last name: Admin    
-   User logon name: sqladmin (or as per your naming convention)    
-   Set the password and configure any other necessary user settings.    
-   Click 'Finish' to create the user account.    
***
![](https://lh7-us.googleusercontent.com/9pAVHwtBBv7JG5leWQTDSKYoch5iwxWe5y2RYPIMRgvCKpionKKdFQorJTw-pH2ROCLsCCbwUWVzWM_uUEha10bHi7jSUigPV3jfi-0bBmf_ljKRvDDBdB9ONyYOKNGNsFfJFlExYc1Qv8oDjNX2VJE)  
![](https://lh7-us.googleusercontent.com/9tQ0EvAhkByScYs9uYrXVZiu9LSsx7MuLPDqT-SsrTZXVph-AjnlqT4GElm8ufnqUH5KP7DcCELa9wOQU4CjGiz_PlQQ7IEgblqANl5CrvyLD-itcQqD3-pRc3SBNBZf-yIgrK5eNLZN6uS6lwtM69g)
![](https://lh7-us.googleusercontent.com/oPbDRda4uTSpwIzmTvEiOldmmremJlzwyEt0Dz3lAtaenjPiGTJ-gF7EIQXiPMnOCIiHp8o7SYxqpFUhimu_rNRnSuRbosNpNffWjipjsETKiDCDIBslJE0xr7VKLEm95CkhSRtfFpjWr3zcLGPHdMA)
-   Add "sql admin" to Domain Admins Group:    
-   In 'Active Directory Users and Computers', locate the "sql admin" user.    
-   Right-click on the "sql admin" user account and select 'Properties'.    
-   Go to the 'Member Of' tab.    
-   Click 'Add' and type "Domain Admins" in the object name box.    
-   Click 'Check Names' to validate, then click 'OK'.    
-   "Domain Admins" should now appear in the list of groups, indicating that "sql admin" is a member.    
-   Click 'OK' to save changes.
![](https://lh7-us.googleusercontent.com/QhfZ5x7cQsgE8J9nybODmrsTaiKSUD4EJFeL3FN47aubmKaHSPi1v8h91IE-c4qpXh_IWu4uHR_OdVJEtqCQTcmCQei8Qnhur_7mwU33OFGmB9dlYdATWoBxfaE6a5TgdKvG3qJL0gMdWkZiLc1BIew)  
![](https://lh7-us.googleusercontent.com/ohpQ5l6v-uVe0aH3p2GEIW7q_TBmNryRpBoIBf2zMvF-3NQlElp3dKGkhfnvgwTADQS1ETBKL6ks7itisQN2dDOKWBzlcYjVxddTHXPuaW-6-W7cFXxBVHiykvor0EYz9ItcRkg7iQPtbLPUdDRZ1xQ)
![](https://lh7-us.googleusercontent.com/nQcy-0Vykj4YivMIpPkl4eKKQhi8tOIBNGeNdT0ouXimqNE3pk4KypqWzQwfiH62IT3MiqbWSjdmFLVXvrO4Q3hdEv09HIbWaB0Dis0pIsh-YWFJ4aXeCNQffvzLNeALbALm6i-_0VlbyTKTJNvifG8)
Verification:
To verify, open the properties of the "sql admin" user account and check the 'Member Of' tab to ensure that "Domain Admins" is listed among the groups.  
***
**Steps to Configure DNS IP:**
***On Node1 (SQLNode1):***    
-   Log in to the server (SQLNode1) using administrative privileges.    
-   Go to 'Network and Sharing Center':    
-   Open 'Control Panel' > 'Network and Sharing Center'.    
-   Access the network adapter properties:    
-   Click on 'Change adapter settings'.    
-   Right-click on the network adapter connected to your network.    
-   Select 'Properties'.    
-   Highlight 'Internet Protocol Version 4 (TCP/IPv4)' and click 'Properties'.    
-   Select 'Use the following DNS server addresses'.    
-   Enter the IP address of your DNS server (e.g., 192.168.254.100 - which is the IP address of your Domain Controller, DC-1) in the 'Preferred DNS server' field.    
-   Click 'OK' to save the changes.
![](https://lh7-us.googleusercontent.com/2wJfQGOBuv61NByPrWiwW_JTBPLl9JE9H7JJ8BwmxuXZ_MBmtz-v1vOknKDMxbxpWvoo1ZuF0_KLd6huTIkF7mTGOzxk9aH_xOAPsUzzDYqPlyKTZRYNOt-iu86zkUXx5OlYXymQ2JZYMn_jPUHVuNg)
***
***Steps to Join Domain:***
On Node1 (SQLNode1) and Node2 (SQLNode2):
-   Log in to each server (SQLNode1 and SQLNode2) using administrative privileges.    
-   Access System Properties:
-   Right-click on the 'This PC' (or 'My Computer') icon on the desktop or in File Explorer.    
-   Select 'Properties' to open the 'System' window.    
-   Join Domain:    
-   In the 'System' window:    
-   Click on 'Advanced system settings' (located on the left-hand side).    
-   Under the 'System Properties' window, select the 'Computer Name' tab.    
-   Click the 'Change' button.    
-   Select the 'Domain' option.    
-   Enter the name of the domain you want to join (e.g., "homelab.com").    
-   Provide Domain Credentials:    
-   You'll be prompted to provide credentials of an account that has permissions to join computers to the domain (such as a Domain Admin or an account with appropriate permissions).    
-   Enter the username and password for a user account that has these permissions.    
-   Click 'OK'.
![](https://lh7-us.googleusercontent.com/rOysI0uyfUD7T4MXurz9o3xiG7ezt1YogcSzVb-2xQF86BxeDzachnTYAJbfcbSV2YzdKQNekqA4Qt4FfCt880i02RSBX4U5cILXMe6oTMiUhS2toeyrNan-8BzfUlkPusEPhKCcJ0B648xkmrFcSR4)  
![](https://lh7-us.googleusercontent.com/vH-1YyS7TbbyT4CwzpBP3V0ybq_8n_lohIybwHFziKr3JduB0zGP-gPZaX8gCaDYEJKJBXubhAmtiTS6to5QlMmWuE0qUFRrGLpUsZGTFVpoWgHk8hBSoexHK0OtqeJcDPC57730BCJfa_luBYdzQFM)
-   Restart and Apply Changes:    
-   You'll receive a message confirming the successful joining of the domain.    
-   Restart each server to finalize the domain joining process.      

 **Verification:**  
-   After the restart, log in to each server using domain credentials (e.g., homelab.com\username).    
-   Open the 'System' window again to verify the domain change:    
-   Right-click on 'This PC' or 'My Computer'.    
-   Select 'Properties' to confirm that the servers are now part of the "homelab.com" domain.
- One can verify the same from domain controller
![](https://lh7-us.googleusercontent.com/iH_D3rSkaYT1XCUDhO1Z2Ye-C4a5LH_26mRaOBnUtIVa_B8E_SKvhKRFunu68aCuiviJT0EdTPzR6Sy9FBJaxqQ-BcEZc-jlmrLeBlf_YPlmqSQ6tM6OdH_FQqDjFkUiSr7mPsKiAdPqV47LZCL5sw0)
***
**Steps to Move Nodes to "SQL Servers" OU:**
-   Access Active Directory Users and Computers:    
-   Log in to the Domain Controller (DC-1).    
-   Open 'Server Manager'.    
-   Go to 'Tools' and select 'Active Directory Users and Computers'.    
-   Locate the Nodes (SQLNode1 and SQLNode2):    
-   In the Active Directory Users and Computers console:    
-   Expand your domain ("homelab.com").    
-   Navigate to the default "Computers" container.    
-   Find and select both nodes (SQLNode1 and SQLNode2).    
-   Move Nodes to "SQL Servers" OU:    
-   Once selected:    
-   Drag and drop both nodes simultaneously (or select both, right-click, and choose 'Move') to the "SQL Servers" OU that you previously created.
![](https://lh7-us.googleusercontent.com/937oK6bPJBEJ-AwE7--z0-mJNZpkYtuCMv80EyZFQY04N5D2QjE7alCX34sncp2GrYls0tLjalvdz34-BXaci-hgrTcRKd4wLhqFiVwTd7WOmZDwviO3Limfh0ZNvIZ-BqL9cHnZ1NXMzXO8VMSAvV4)
***
**Install Failover cluster services**
Before installation of failover cluster services provide full rights to both nodes in domain controller
**Steps to Grant Full Control via Security Settings of a Computer Object:**
-   Access Active Directory Users and Computers:    
-   Log in to the Domain Controller (DC-1).    
-   Open 'Server Manager'.    
-   Go to 'Tools' and select 'Active Directory Users and Computers'.    
-   Locate Computer Object (e.g., SQLNode1 or SQLNode2):    
-   Navigate to the location where a computer object (e.g., SQLNode1) is located (for example, "SQL Servers" OU).    
-   Grant Full Control Permissions:    
-   Right-click on the computer object (SQLNode1 or SQLNode2).    
-   Select 'Properties' to open the properties window for the computer object.    
-   Go to the 'Security' tab.   
-   Add Permissions for Another Computer Object:    
-   Click on 'Edit' or 'Advanced' to manage permissions.    
-   Click on 'Add' to add a new user or group.    
-   Select Object Types and Search for Computers:    
-   In the 'Select Users, Computers, Service Accounts, or Groups' dialog:    
-   Click on 'Object Types'.    
-   Check the box for 'Computers'.    
-   Click 'OK'.
![](https://lh7-us.googleusercontent.com/-cN_oRoK17NOALQP5o9jHMf8F1AP0-qjt6S6JP-bOblGjWXU8WCGMrO0gbVIQrp5LNZph6ls_sRnhn_PVbGJp5NjU38JcB6RTDfEiYf6q6UE7xNiiATpqUc73C6NZ6vF_G-UF8sBmlh2ENp3Xo-GEbg)
-   Search and Select the Other Computer (e.g., SQLNode2):    
-   In the 'Enter the object names to select' field:    
-   Type the name of the other computer object (e.g., SQLNode2).    
-   Click 'Check Names' to validate the computer object.    
-   Once validated, click 'OK'. 
![](https://lh7-us.googleusercontent.com/8gapkB_7OmHCidM5wZiyb2DtuxcQQw1v61drUztBuqDENEDCqI8y4cyM5ljw7ybt960JZ7dEne0hbQIHfCKAO9Z-HIH0lkCEH0tKX2Mn6Gc6swozwQT2E4zA33lDi_Qocfar4urZREqRvlhAB4bbHxM)
-   Assign Full Control Permissions:    
-   Back in the 'Permissions' window:    
-   Select the user or group you added.    
-   Configure 'Allow' permissions and select 'Full Control'.    
-   Click 'Apply' and then 'OK' to apply the changes.
![](https://lh7-us.googleusercontent.com/A9WjtBQesolA6FopwJ3GyeHoT-zyNIr2TTsGkWm5Q-IQQ19Sh8HPgY85ABEKjz_dPSoeZzS_dbSXcuRiy3JRIRTF7ObVfVgMSrgYghPpNXKBudCn2_BoVZ3ujlDENmxTi_dFgvXk3OSOKKxIm21uILM)
***  
**Steps to install failover cluster feature**
-   Login to Node1 (SQLNode1) and Node2 (SQLNode2):    
-   Use administrative credentials to log in to each server.    
-   Access Server Manager:    
-   Open 'Server Manager'.    
-   Add Roles and Features:    
-   Click on 'Manage' at the top-right.    
-   Select 'Add Roles and Features'.
![](https://lh7-us.googleusercontent.com/CDC1KHLQZi7KtX7vGrUmiRexNuGVw9Aaw7bYjm3yYVtRqqMuoWyufrTxLL3GoYE9Y9Ip19Z4cROA4GecSnoJnGZMnD2p8oAyOLt7OKqMH5y6h7LqidrWJ6YHUKHo-4ZWWf2EKTYbzCjca_T2hvD_xE8)
-   Installation Type:    
-   Click 'Next' on the Before you begin page.    
-   Role-based or Feature-based Installation:    
-   Choose 'Role-based or feature-based installation' and click 'Next'.    
-   Select a Server:    
-   Ensure the local server is selected and click 'Next'.    
-   Server Roles:    
-   Skip this step by clicking 'Next' as Failover Cluster Services are not a role but a feature.    
-   Features:    
-   Scroll down and find 'Failover Clustering'.    
-   Check the box next to 'Failover Clustering' to select the feature.    
-   Add Features:    
-   Click 'Add Features' if prompted.    
-   Confirmation:    
-   Review the selected features and click 'Next'.    
-   Install:    
-   Click 'Install' to begin the installation.    
-   Installation Progress:    
-   Wait for the installation to complete. It might take a few minutes.    
![](https://lh7-us.googleusercontent.com/e4nx7LwOjZoEGMqmhF2pf5aktBjdxhinoH8bcSEMCMJ97SM1unO4UEKzmsD-Gg-wrkqXAWhF6pdkgC6oCtjwJNJtrohUrKw5jUnClYVD8Ly94nJMo1VS-MaxaFm4Pc0Mjo8s9cGh7Hy-AohJAoMToa0)  
-   Restart SQLNode1 and SQLNode2 servers.    
-   Log in as Domain User:    
-   Log in to Node1 (SQLNode1) and Node2 (SQLNode2) as a domain user (e.g., homelab\sqladmin) with administrative privileges.
![](https://lh7-us.googleusercontent.com/GjwlHKYFTieWnzUlKTUoerLULnHzB_TgL7aXaxxTTD-S4NZWDOaQ61nq_egKQuLzpaQ51LrqYiMvVnox9BFxnY3cclV903RXZ3TSboPUBryGXK6COO189g57optkEK2AxNCdR1vayIV5LFVn79XHYOo)
***
**Validate Configuration**
***Steps to Validate Cluster Configuration with a Specific IP Address:***
-   Launch Failover Cluster Validation Wizard:    
-   Open Failover Cluster Manager.    
-   Navigate to the "Failover Cluster Manager" node.    
-   Right-click on the cluster name or node and select "Validate This Cluster."    
-   Begin Validation Process:    
-   Select the tests you want to run in the Failover Cluster Validation Wizard.    
-   Click "Next" to proceed. 
![](https://lh7-us.googleusercontent.com/5uDA_bv4r1JSA0z9YlyDd7KhWu-eov6jDKQ6iYqnAY2wkGT2Mh26AU4fXRMJMYQzHgOMuXAa_-k07hG9nDimeNqBwyBC30Tvp7lGTp3fhnnMheD2yVG0wQhZLHX0147ZYkYjlgZmowyKp8t7A1DiNj0)
![](https://lh7-us.googleusercontent.com/qUNe4h1OQCReN2PK92Ayt_q1jZaLA5gjlcnTnAmJd5N2SWV3UsOElDOfcq06P0V9k15S3BAY-1raitGCFRN2qSQKq3qlvkXUC9uZKH7TKSLLn0sdTHSn1ziw-aXNJm3Ycsu-7JL_JHxPminr_ZrY5b4)      
-   Specify Cluster Name and IP:    
-   Input the desired Cluster Name as "SQL_WFCS"    
-   Set the IP Address for the cluster as 192.168.254.4.    
![](https://lh7-us.googleusercontent.com/NvUTY5o-hoK4uo8hCVHXxJAE16EP9y77N8vC8KJDxqO-NvgUnxq5BV_KnUGdAW7zKa0brw7Acif9Do0W6VifTWzl2C8wO_K1h8NK_SK0OqTj7MNZub5Dbpbt0VEwHdQNtstBVTXfCvW0rKWluOGN86c)
-   Select Storage Tests:    
-   Uncheck or deselect this option to exclude adding all eligible storage to the cluster.    
-   Complete the Validation:    
-   Continue with the wizard by following the steps and select any other tests relevant to your configuration and needs.    
-   Review the summary of selected tests and configurations.    
![](https://lh7-us.googleusercontent.com/PBj5dOY72gtUM9kVGFcT3c49vgaOx0g9fpJLjl3wp5l1McDnKunrnJlbnuQUzRqWSVVh0TnumfnDpHvd0xGb2nTrYn969YsE9a3tSAERCgN_EETlT2LRoXJ-shhGITETmasRYn6pjwm0VsttogLbLwE)    
![](https://lh7-us.googleusercontent.com/Jq05_m_9ujriE8mQ3c-s4LX90h8A4MywANa5I4lKYeWLDj5NZKjzzu6NLPHQxGBa3az6-mnNiLhr9ixRB2yQqknKwzhmsyI1gGCeUuHTd_DHFQeKKH3bNqxwZp2LfyjH5nsMALFZAMAT9xvinwiZwcA)
***  
**Grant Full Rights within "SQL_WFCS" Group:**
-   Access Active Directory Users and Computers on DC-1.    
-   Locate "SQL_WFCS" group within "SQL Servers" OU.    
-   Provide Full Control permissions to SQLNode1 and SQLNode2 computer objects within the "SQL_WFCS" security group.    
![](https://lh7-us.googleusercontent.com/H3b_ORj0drObhKSVDZqYbAK0BtDg8c3rxZy4Fp4GkX55byllTIRwkb2G-SnB9fmzt5BZA555mgDU6C9Kxcp_fxwSR0hp47bXcEfHmlIZX7SWq46or9hBIo6c8GJGz9ryBp02hhiC9a2a0aVAWhdNT0U)  
![](https://lh7-us.googleusercontent.com/gA7rC72ERZJpShWybe4gGlnLGJi51SwAElMCqZsQMI34tOP6Wer0-f0Kcpc9V0gUWL7QqVE_EryQ45CHSmc8GRmlQc-H7uxDGINIEZDcqkzC7yCs6-mOOpsY69LeKsTQHt9RyFsicRvXr-9cpsoObSI)  

**Test windows server failover by creating an empty role**
-   Access Failover Cluster Manager:   
-   Open Failover Cluster Manager on SQLNode1 (Node 1).    
-   Create an Empty Role:    
-   Right-click on the Cluster name or appropriate resource group.    
-   Select "Create Empty Role" or a similar option to create a new role.    
-   Configure the Role:    
-   Assign a name to the role and proceed with the empty role creation process.    
-   Leave it empty as this is a test role.    
-   Verify Role Status:    
-   Ensure that the role is online and functioning correctly within SQLNode1 (Node 1).        
-   Initiate a failover manually from SQLNode1 to SQLNode2:    
-   Right-click on the role created in Step 2.    
-   Select "Move," and then "Best Possible Node" or specifically choose SQLNode2 (Node 2).    
-   Monitor the failover process and verify the role status in SQLNode2.    
-   Verify Failover Success:    
-   Ensure that the role has failed over to SQLNode2 (Node 2) and is functioning as expected.    
-   Verify that services/applications associated with the role are accessible and running without issues.    

**Delete the Test Role:**    
-   Once the failover testing is successful and verified, delete the test role:    
-   Right-click on the test role created earlier.    
-   Select "Delete" or a similar option to remove the test role from the cluster.    
***
**Install SQL SERVER and SSMS on Node 1 and Node 2**
***SQL Server Installation:***
-   Install SQL Server on Node1 (SQLNode1) and Node2 (SQLNode2):    
-   Install SQL Server in standalone mode on both nodes.    
-   Choose default options, evaluation edition, default instance, mixed mode authentication, and add the current user for Windows authentication.    
![](https://lh7-us.googleusercontent.com/aunvj2EWFKppU-qX83srMaIiR0vy1OXA1ZGmuNzuSC3yIvJWd3DQNCXleCovfONkk1VFSdwDUTrELJZDqXfWP639S3W_Q5puJAUFg9SGX8f64fB9QDByQbvnQWrUdC7vUqABOb7s_H4C3MSbUC18c-M)  
![](https://lh7-us.googleusercontent.com/G0gGAxIK7_rrt82ph12fe80TobNrHKmdkHGM6v2GJHzv8-tlNKbw83UoCC2bbSZdv7__-Dl2BRgGE6Opbzbo7Md8SEvgTG2XGy8D6eLA2hLla3dTMrhaWONUmUxf0HiUm67-5rHID3e7metlyCeVJoQ)  
![](https://lh7-us.googleusercontent.com/npV8X9tYUsrzVKGpM-fJBJUVlODF4tGcSz9Gz_IchkchhDAbQ_UyATVRc_m6PALWUTJaAZimkHm3iW7FC_KRHht9Oh08KmgHlf0LxZtCS8PUX07jA70ePBjGhx5Qn7_bTgGbud9kLDUJJm-0yd38v50)
![](https://lh7-us.googleusercontent.com/F04DF5_gDGiIpxToLeXwDTSSY1xNWYUGkq1cuq0bgr7jshrJRKiNyI0AoU6oO-UGVSbsZLk89_WVLAMdEwk3ZOoPk0A3gNwGX3qqaSRdKP_GStGI993qBYgNHVBI5CFPcUy1gLrApj00V1Z7fHxIJ3w)  
![](https://lh7-us.googleusercontent.com/g6pxdbaPyH7qF2rYJCo_89aT5XebfqySqNw5TVTaWYp27V9VOk4VD2Ch0wY4aQ-FYlVFuLnR5SZYKc5oDAbep_VGN0EAjLWGIcprtKzbAhaV7esasX_OWKpYB25t3O6F495zpPu6Gv_JD_h_8-i9J8Y)  
**SQL Server Management Studio (SSMS) Installation:**
-   Install SSMS on Node1 and Node2:    
-   Download SSMS from Microsoft's website.    
-   Run the SSMS setup file and follow the installation wizard on both nodes.    
  
***Adjust SQL Server Agent Service Startup Type:***
-   Change SQL Server Agent Service Startup Type:    
-   Open the Services application (services.msc).    
-   Locate "SQL Server Agent" service.    
-   Right-click, select "Properties," and set the "Startup type" to "Automatic."    
-   Start the service if it's not running.    
![](https://lh7-us.googleusercontent.com/EwLtdFiOGXz-J6zKoj0vikQ98TXQPVWyYCF7mSwQN7tCNx-XFaze8l24vvkhgGUNiYY8OupSxnp_gzPp1-CazVF0dELVjcN2Wvzg9faMRixT917jxpzxQmc6jPzwe_Z3p4SMbyL3X1-ofhZfYj0THYE)  
***
**Configure Always On**
***Enabling Always On Availability Groups:***
-   Open SQL Server Configuration Manager:    
-   Launch SQL Server Configuration Manager on Node1 (SQLNode1).    
-   Enable Always On Availability Groups (Repeat for Node2):    
-   Expand "SQL Server Services" in Configuration Manager.    
-   Right-click on the SQL Server service (instance) that you want to configure for Always On Availability Groups.    
-   Select "Properties."    
-   Enable Always On Availability Groups:    
-   In the Properties window, go to the "Always On High Availability" tab.    
-   Check the box to "Enable Always On Availability Groups."    
-   Click "OK" to save the changes.    
![](https://lh7-us.googleusercontent.com/QbUUz-r4XUppzyu5ydtSzw2rYhGg_JHMD8eXP__tAr43UjwuGwthSvCKW-Oq18X_GyOOHJH4l4ccltLkfyOyWs8WrCPQbeOCxwE4h3pv1WzDJxoBy_EF_lcoP9yJwATtJlumxlkXR78QWYRxavw-wZA)
-   Restart SQL Server Service:    
-   After enabling Always On Availability Groups, restart the SQL Server service for the changes to take effect.            

**Create a new availability group**
***First check connectivity between two nodes using ssms***  
**Enable Named Pipes in SQL Server Configuration Manager:**
-   Open SQL Server Configuration Manager:    
-   Launch SQL Server Configuration Manager on both Node1 and Node2.    
-   Enable Named Pipes:    
-   Expand "SQL Server Network Configuration."    
-   Click on "Protocols for [Your SQL Server Instance]."    
-   Right-click on "Named Pipes" and select "Enable" if it's not already enabled.    
-   Restart SQL Server Service:    
-   After enabling Named Pipes, restart the SQL Server service for the changes to take effect.      
![](https://lh7-us.googleusercontent.com/BQZBIsEAfhzycD2E17HFwLjQtRl98ATPuBr3RKQ770Ksj0-xOG1mj43-R1b8Utt8yDcHFqkUrlOv9XI7YAeBFScqnoC7IwCCC40kJzl4PBP80xAMmx360E0MPdhTBG7sUuQY26fJ5ntoBJZycTVK_Rc)  
**Add Inbound Rules for Multiple Ports and Protocols:**
-   Access Windows Defender Firewall:    
-   Open Windows Defender Firewall settings on both Node1 and Node2.
-   Add Inbound Rules for TCP and UDP Ports:
-   Click on "Inbound Rules" on the left-hand side.    
-   Click "New Rule" on the right-hand side.
![](https://lh7-us.googleusercontent.com/dV2HMxVjujBVdqgOGvFGUrvhmY60hbf3kEuqhRGFBGa4vXZef1ydoitqejSZvBMtU0LBRRZfZ4TlodQhOpgGCvQX8rBlYvLjg7nkAgTktRBFM5yTFqS0TkWC8hmf90Lj7r7uQJEZ4ERCjkGFgmTTEXk)
-   Create Rule for TCP and UDP Ports 1433 and 5022:    
-   Choose "Port" and click "Next."    
![](https://lh7-us.googleusercontent.com/2EYA98Jjp3iHI0w-1KpC1sKymvb3_8RP14OJC4CFunQkHoknaDyQIR10reCeDQidRuCRxwcvWD7ww0muR0-ncXj7l8-isu3w_KPe7AiPry5EV0dy_bvO805M3FQTmVyhuKDTf1aunqZJZn18MRkfLXI)
-   Enter ports and protocols using comma separation:    
-   For TCP: 1433, 5022 (This will create rules for both TCP ports 1433 and 5022).    
-   For UDP: 1433, 5022 (Similarly, for UDP ports 1433 and 5022).    
![](https://lh7-us.googleusercontent.com/3Jif8t1esbxvMyliXmoAf-U8NwF4tU7xVAelIQ3pAUzEGdXSQo0oVX_g8ODcqzHmHu5xnnEbHqVgRMNVrCq9soiFn1WZxnOAs_kqr4yFnZJCCj_Mqlq2lRIWbal9bZPIua9GAny3OtM-L8uaPh85GVw)
-   Click "Next," then choose "Allow the connection."    
-   Select the appropriate profiles (Domain, Private, Public).    
-   Provide a name for the rule, such as "SQL Server TCP/UDP 1433, 5022."    
-   Click "Finish" to add the rule.        

***Test Connection:***
-   Verify connectivity between Node1 and Node2 by attempting connections using SQL Server Management Studio (SSMS) and executing test queries.    
![](https://lh7-us.googleusercontent.com/H0NMAYQOg8nHBcf_iktjnGmuUd37VT9vBkmWr2MKoELlZeMPGE99o-So_HTbB9z779gaQrCx1gQ9NZscPTQpJgbAeuOvc3EOwPVKGSN_rQBK-ZebD1YSjLXuZ4eoob218xLsDlHXPGv9LM8a9eRJaJk)    
  
**Change the log on user for sql services**
***Steps to change the logon account for SQL Server, SQL Server Agent, and SQL Server Browser services:***
-   Open Services:    
-   Press Win + R, type services.msc, and hit Enter.    
-   Locate SQL Services:    
-   Scroll down and find the following services:    
-   SQL Server (MSSQLSERVER) or your SQL Server instance name.    
-   SQL Server Agent (MSSQLSERVER) or your SQL Server instance name.    
-   SQL Server Browser.    
-   Change Logon Account:    
-   Right-click on each service.    
-   Select "Properties."    
-   Go to Log On Tab:    
-   In the "Properties" window, navigate to the "Log On" tab.    
-   Change Logon Account:    
-   Select the "This account" option.    
-   Enter the domain and username in the format homelab\sqladmin.    
-   Enter the password for the specified user.    
-   Apply Changes:    
-   Click "Apply" and then "OK" to save the changes.    
-   Restart Services:    
-   Right-click on each service and select "Restart" or stop and start the services to apply the changes.
![](https://lh7-us.googleusercontent.com/fYGZMp6DOr2e9xKtp8zmyb2-jDT1m19HC-_gcL6-WD-lfX6CT0w0BpFOaERjbcbg6IUCGqxbwC86mJx2qnWTbu5UPz1BrvzylfwjzlXhMhRwm0vgZENPJgTvdH1VwzbAmnFuLSyC5854uqfF0Xwibrk)
![](https://lh7-us.googleusercontent.com/SWVYNl9sQPCgvlI7xawvy_nzaBdNyT_WzYJUH-lNACDd3AzlU6HLs59s8BMrzf9V7xjocGHN-KsMpy5cuUX-ofIY09gUyn0Lk4ndf48_-pXYJl4HJrSBbpbLomp74Me_P9pDwR_sEqpvQDMglnA19QY)  

*Before creating availability group create a database and take a full backup*
  **Creating Always On Availability Group (SQLAG):**
***Create Availability Group:***    
-   Right-click on "Availability Groups" -> "New Availability Group Wizard."    
-   Enter a name for the availability group (SQLAG).    
![](https://lh7-us.googleusercontent.com/5jSy5NbVGyEDBEUqh9kZtyqsXc1o-U5sCInDz1EkxjTQn22FuAEsFLOgOqu3euD0wej8haaQ8DsIEsF7Wos16yCIfLEn-_VqpxVeX-8R38NiDU3eIV5anFUsCUnVP_Jz0DYw7QmSWvFVw7kvOhKFCow)
-   Add the database you previously created to the availability group.    
-   Choose "Automatic Failover" for the primary replica.    
![](https://lh7-us.googleusercontent.com/WX7sMwu6EyWNfX_n_7uEjKQVPZWjLyVl5vhpVPXsQdczjwyIJRHuQ_mQPvCI0Ll_sOuprHpS7-nCgZVpmcHA-cAwcMI222oYCvoxKSPg0u4M5z5ZauhVicaXgLxeImdUe3rh7Gnp55EKJmv2Id4LjZ8)
**Add Replica:**
-   Add the secondary node (SQLNode2) as a replica to the availability group.    
-   Select "Automatic Failover" for the secondary replica.    
![](https://lh7-us.googleusercontent.com/CYUvqwud8TFiwzduTSUUTF_IBmn-7KNqNKg0YMhzzhzcY79r65rZIw9nhBwbqdZKaGHVcz7DGaEKGkJxSek_pddT2J525WLuEEQo6VgAdM7IzumYGsYBEk8miawMgQ6GSE3G84-8BJXwfBm9VADCGos) 
![](https://lh7-us.googleusercontent.com/LP-Xnwx9KaSni9ZqybGxx1-JAahDnYwtxuu_Z4lYN7FTnjDjUH3nqBE9n6HlYAHrXLW6UBVHkdhL9LdjhhRHYvLY87r9dIunAdD-WbTzRSe4Y0xzM2Epkc464VGqJOVQ92uc61YcDlBFMECijVnobno)
-   Set Backup Preferences:    
-   In the wizard, set the backup preferences to "Primary" for full database and log backups.    
-   Set the "Backup Priority" as per preference (Primary/Secondary as preferred).    
-   Configure Shared Backup Directory:    
-   Configure a shared directory accessible by both nodes for storing backups.  
-   Ensure appropriate permissions are set for both nodes to access this shared directory.  
![](https://lh7-us.googleusercontent.com/E-tHgc67iVftK0es_TIV1RsEI4_VUCrPeDO2n7PJebCGxs6fVqeeP49yqTpGA0rwjyBfwJpVSFtEJYi3qL2OVLKB7TTT2kXssEdWOHrjMNMTZBOQnSGupl-1rO6_tzscEogAopclo_9TW6tbVjZy46A)
-   Finish Wizard:    
-   Complete the wizard to create the availability group.
![](https://lh7-us.googleusercontent.com/EDFTJCbHRviNqRewG4Nu8_sCgjJYyNfCMHZFg9Hi2Wa_jpMYP5OBvoxMT2ydA4gTpSrmtEem6_aurq52EFDtQKdMLLYMuVXBBZ2W3CLqSz3XaSth8PNpEzgesNXaGODjaL6q8nLWCAJhgXdfT7K4r6Y)
![](https://lh7-us.googleusercontent.com/GPk2OM7nYQfxTR5NWWdaRWgHxqtEeTDt5kbjq9vskVPR9-S51SjXwdiy_1I5AF4kqBvKAsX8dfA3XOBsj7WReC6k5tx-S3Grc_BaU0RnCWP8QxwbGx3mpSK2UBtyPc4LS0_-sauiYjb0CQZRIM8LRyA)
![](https://lh7-us.googleusercontent.com/uF7c9RCS2-qqXybFBORHZBgm6UhOEl4J6_Zf57CQXpes1dkn_hal50fSgV8lgUVeJiFA2HdefBy6CKnlUZXdi9nR_IuloH9RDQx0gRPWvTqvOCfwAsQ5D64rs8qpO3mgfsGCD9Bs8Ewd--8gWG6_L0w)
![](https://lh7-us.googleusercontent.com/S5nQFiGyeVQFUCXgIMRyORQ9VyoBgR3BZTZTn8yyKm0_-jWbVrnRpP2cnWMgQPfwknBSWTcOG_tS4JyYkAfdhJZvydcGMRX5gNyDV5YkHOe_U-CULE4fqOokjpH9RRp_1z_CV2QCMzGIn3oZEAxLfpM)

**Adding listener**
***In DC1 add DNS Record***
-   Open DNS Manager:    
-   Go to DC1 (Domain Controller).  
-   Open DNS Manager.
-   Navigate to Forward Lookup Zones:
-   Expand the DC1 server.    
-   Expand "Forward Lookup Zones."    
-   Locate and expand the zone for "homelab.com."   
-   Add New Host (A or AAAA Record):  
-   Right-click on the "homelab.com" zone.    
-   Choose "New Host (A or AAAA)."    
-   Enter Host Information:    
-   In the "Name" field, enter "sql-listener" (without quotes).   
-   Enter the IP address as 192.168.254.5.
-   Finish Adding the Record:    
-   Click "Add Host" or "OK" to create the new host record.
![](https://lh7-us.googleusercontent.com/9wD-x48OPPhGEP0CubRAH5vs8oKMHilxG99c1fhj2m1uBxduxlNwzhq44CdPOYTkkW8EwGoplSrE-xbNeMQ-ZIIIl2iUoBrANEC2x8YPiyAVW5oe_bGwxYOC3JVNfajwL_GPtFGGQsl4xRuFctdDkh8)
***
**Grant full rights to the cluster (SQL_WFCS) on the "sql-listener" computer object**
-   Open Active Directory Users and Computers:    
-   Access the Active Directory Users and Computers console on your domain controller.    
-   Navigate to the Desired Location:    
-   Locate the container or organizational unit (OU) where the "sql-listener" computer object is stored.    
-   Grant Full Rights to SQL_WFCS:    
-   Right-click on the "sql-listener" computer object.    
-   Select "Properties."    
-   Go to the "Security" tab.
-   Add SQL_WFCS Cluster:    
-   Click on "Add" to add a new user or group.   
-   Type in "SQL_WFCS" and click "Check Names" to validate.  
-   Once verified, click "OK."    
-   Set Permissions for SQL_WFCS:    
-   Set the permissions for the "SQL_WFCS" cluster to have "Full Control" or specific permissions as needed for the "sql-listener" computer object.    
-   Apply Changes:   
-   Click "Apply" and then "OK" to save the changes.    
![](https://lh7-us.googleusercontent.com/C04jbOYIPm0YH_ozQD_ubuqgJB5OQ_7SnDg4oP7n-VJAblflyi0dPl1Fg5qqw5z03V0XWJLlK9IVk2wrH28cT5AJX2ZzeMWd6d0yzD1rZXNtx54Ii59erUMIxXGbpFB2UDfiEg9h7IBUbG6f58px0e0)
![](https://lh7-us.googleusercontent.com/gaqqo9DG870fwn05_obrBrnOZaiE9jfBYq3LNIiN9dNtwsuvNSN2b4OrDlIlzxkidXV9CVAQUsU_iJPr_KlHMoGW-DB28IUiy8PRpvypNkjxdscRAgfxhV1MlCBC7bbf1aFmBkamdiIP4Uta6TH1c3A)
![](https://lh7-us.googleusercontent.com/pbEBa38yWUmBQKqzUSabe3qUeUEe3lZQ2Zz_4n9Zge_ujnMwZtWI8-vqE8C4MkHHad63H1-in4CIAVXmGGOXDX_dj2PV9eDHk3d5JW8KofhdgqOmh4Gh4qQDAl6NQk95mHep_uykPG4BWOoYPz5K3iM)
![](https://lh7-us.googleusercontent.com/tRYB96dtvtkQ2Y4RLy-OzCN_SZVh1wqsakVPDaSIvCusJZdTj8ZkuDhlSXh_kIQkxLLti5UakTKhqdLTOxbdEDkgLxlaoEPR_mkpIUIXEfik5CIknKhfReJP2B5x3Ip8WMoTaGmOm_LHRpvMwMOr8AY)

***Add listener to availability group***
-   Open SQL Server Management Studio:    
-   Launch SSMS on Node1.    
-   Connect to the SQL Server Instance:    
-   Connect to the SQL Server instance hosting the primary replica of the Availability Group.    
-   Navigate to Always On High Availability:    
-   Expand the server in Object Explorer.    
-   Go to "Always On High Availability."    
-   Add SQL Listener to Availability Group:    
-   Right-click on the "Availability Groups" folder.    
-   Select "Add Replica to Availability Group."    
-   Join as a Listener:    
-   In the wizard, choose to join the existing Availability Group.    
-   Select the existing Availability Group (SQLAG) that you've previously created.    
-   Specify Joining Details:    
-   Provide necessary details like the listener name ("sql-listener"), IP configuration, and port.    
-   Use the previously created DNS entry for the listener name ("sql-listener") and specify its IP (192.168.254.5).    
![](https://lh7-us.googleusercontent.com/WVxGUAuGsaDkEb1wk9fd0GdYX2IR5zH1moEyA1jmTVyAMbr_J_C-RGcs64Ur8LVr7LC0p5IubAQ02MfxhU0cDmO4y2UA40WMgIzPx4xSwbqteOCYryhyn4o9S3l1ySgsjJBVknXktYyXyPlKsiYAeGI)  
![](https://lh7-us.googleusercontent.com/Tn00voEeexydk8xFtRb14bOSs7D5gZVQC3IBY_ju1KtEpS2yIsweeQpc8R41pDQXK_3wNOZyryQ-vOqTmYaCG0lfOlY7TnznHeEZ99EyWe7m2ma4LpXCFENq4aEr3gVUc0ghyF2F8O7R5I0hE6q5ulE)
![](https://lh7-us.googleusercontent.com/iH8lri_rpHlM4uh2YlpHa_DAsWaGDICTiuYT4vsNLURGeqzgmXcjV95hNontlED_z43OHPrsZfxITelvR0WY15BXr5VObg4EfzFztsWVu9aEFoS0UtjkUcOpEkE7tjgtwB94dDXb8m9dP_0LF6yjnXk)
![](https://lh7-us.googleusercontent.com/N1lTsCgF8Y6ljC8yyDBYLuc8tA_IMO1trFAb7dlrtC0Ptnd30XMa-7yWx3ilo7TOz4DFPhf6bNybv3q2ULQfE3t5IiXc7Z5yq_sXLxhLJnb6V-QcGgMiOUNT4MIjDFnC9_-524p684-Wc7sC4TvFnuA)  
***
**Test Failover**
![](https://lh7-us.googleusercontent.com/jt1bprBwfNfHgVWz-H_bAi2JdHZdfih29kFzr_gsvyQV1MTSasJdnIvY8rPKbHADZ8dBnnWBU05DIE8_9PM1qiembl3GFngKXawbxxzbpy87MKEU10KTI5ZkkiJozwESPyHV75LPXcHkEguUFcH6Jgw)
![](https://lh7-us.googleusercontent.com/Cn8YT-idfkBqVSg4fVpDrCrUdXYFZpELQMQBLXID-dVh9dx7ptQ-AOF2HsWZS7iZc-Bbo6YXwcbe-gVMLidkYaXOYouw7CJBdhwWrjytfFDKczOzGVaHJ4aeECofPr244NoYIQ0Jkg3jvObh6NQoe1s)
![](https://lh7-us.googleusercontent.com/u-da-V4RlfnJ6mw_NTTm22V_W57q5OL776quztS2cdLjFDZZIng7qbtnbCsgoR9MIfnAQhXTuFIB5zF3qxAF71hkA9hBfR8KRWRhFD-9c3S1vFPSN3dYSoatdU_bqz09bOtgCMJEi41yOkk7ym75QMg)
![](https://lh7-us.googleusercontent.com/WQRVEHKR4xR4T7yTSUEnrfg8Z6Z09y90HLFn-t5ixGmxEqbzVr2P0EvRSnojNSSMyf1r3efs1Y_5uFmtx32rBV-p6aEs7EVUuNE5_SWhVqHROPJAoMJiGplgnGw798GTXjPAHXcAmf_kKewWJfKgH0s)
![](https://lh7-us.googleusercontent.com/fnrkghC0eqvq2AkfwTRlGsdLwOMkcZATw00jXZjQHq5-P0tAArrNzAp0g9p7GR80Gup977oqK8tG4ORTgS-sXpa8amoI-EuB9fqIhA1C9yoQM53oayu2Wk3JWLvq-3EpYGYS29jeXa3Y8SbZnnsIDIw)
![](https://lh7-us.googleusercontent.com/EplyTcaUiquREs4VOP6wu4uEvbib19EWQXnupto5nvLrK5ZG2uUXroFvxZIJhgt_aqbmCWyNkjeEeYhikdt3URxwosl8aWAlaU1FFjK_2VTLeQ5LPJbGyllLWrdnzJyeAfjsbubHI3ZtQD6Hj_BDteg)
![](https://lh7-us.googleusercontent.com/Zqq_MMEIzYylQI9zkNgZeXC1VGdypFZjM5pLOwb-dBYQC5-wYe9AiyT_L9DX6mO1eIfrp6I-ETM76xRRO93W5J9fRnLrErwbs2DthwDHEDc9wDcp5Xo6uS2gWVv8crGEXZ11E3FJrgWUTi_eZhRM6Eo)
***
