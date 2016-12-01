- [Installation Procedure](#installation-procedure)
    + [Access to the software:](#access-to-the-software)
    + [Install and launch AnyBody:](#install-and-launch-anybody)
    + [Register your license(s):](#register-your-licenses)
- [Some license related problems and solutions](#some-license-related-problems-and-solutions)
    + [License Status: "Bad signature in license (-5)"](#license-status-bad-signature-in-license--5)
    + [License Status: "No license check or No authorization for product or License server doesn’t support this. Make sure you have imported a valid license file."](#license-status-no-license-check-or-no-authorization-for-product-or-license-server-doesnt-support-this-make-sure-you-have-imported-a-valid-license-file)
    + [License Status: "All licenses in use (-22)"](#license-status-all-licenses-in-use--22)
    + [License Status: "Wrong host for license (-4)"](#license-status-wrong-host-for-license--4)
    + [License Status: "Communication with server failed! Please make sure the server has been started."](#license-status-communication-with-server-failed-please-make-sure-the-server-has-been-started)
    + [How to make the license work for FE converters](#how-to-make-the-license-work-for-fe-converters)
- [Typical Problems](#typical-problems)
    + [It is impossible to contact the server from the client althought the server is available from the network, with service started:](#it-is-impossible-to-contact-the-server-from-the-client-althought-the-server-is-available-from-the-network-with-service-started)
    + [Which port should be used when a 2nd RLM service already uses port 5054 on the same server?:](#which-port-should-be-used-when-a-2nd-rlm-service-already-uses-port-5054-on-the-same-server)



Installation Procedure
---

#### Access to the software:

A self-installing exe file is available [here](http://www.anybodytech.com/877.html?did=anybody.overview).

Notice:
* It requires AnyBodyTM Web-user login. If you are not a registered web-user already, you
must register an account on the login page.
* The setup file is typically called AnyBodySetup.v1.v2.v3.v4.exe, where v1, v2, v3, and v4
are digits of the version number. The setup file for the 64 bit version of AnyBodyTM is called
AnyBodySetup.v1.v2.v3.v4x64:exe:


#### Install and launch AnyBody:
(a) Run the setup exe file and follow the instructions in the installation wizard.

(b) Custom installation allows you to select the installation directory and select components. Notice that the install wizard’s choice of directory prepares for coexistence with older and future versions.

(c) In order to install optional Tool components of the AnyBodyTM package such as the SolidWorks2AnyBody add-in, you must either do a full installation or select the specific tools when selecting components for a custom installation.

(d) During the final part of the installation, drivers for AnyBodyTM’s hardware dongles (HASP dongles) may be installed if relevant drivers are not found on your computer.

(e) Finalize:
i. Choose to launch AnyBodyTM as the final step; this will bring you to Step 3.
ii. Choose to open Add/Remove Programs to remove older versions of AnyBodyTM.
This can always be done later.

#### Register your license(s):

(a) If AnyBody is installed for the first time, or for other reasons no valid license is found, AnyBodyTM’s License Manager will appear when you launch AnyBody.exe.
(b) If you have a valid license already, select Import License File, browse for your license file, and import it.
• You can choose to import the license for all users of the computer, or just for your own user account.
• You may need administrative priviledges during the process, if you install the license for all users.
• If you have a dongle-bound license, insert the USB-dongle while installing the license. The license will be validated during the process.

(c) If you have purchased a license, but have no license file yet, you may need to send host-id information to AnyBodyTM Technology. From the License Manager dialog, you can gather this information:
i. Insert your dongle, if relevant.
ii. The "View host IDs" button will open a dialog with the possible Host-IDs. From here they can be sent to AnyBodyTM Technology.

(d) If you have not bought a license, you can select a trial license.
i. Trial licenses are special computer-bound licenses valid for 30 days. The "Get trial license" button will bring you to the AnyBody Technology webpage where you can request a license key.

(e) Pressing OK after entering a valid license will launch AnyBodyTM.

(f) You can enter new licenses later from the menu Help->Registration. This will bring up the AnyBodyTM License Manager dialog.

Notice:
(a) If you need assistance with the installation, please contact AnyBodyTM support at support@anybodytech.com or phone +45 9635 4286.
(b) A free forum providing support is available at the www.anyscript.org along with a lot of resources. The web-user login you have used at www.anyboytech.com is also valid at
www.anyscript.org and vice versa.
(c) AnyBodyTM’s console application cannot register licenses. It will only run if a valid license has been entered from AnyBodyTM’s GUI.
5. To exploit more than 2GB of memory onWin 32 bit systems, special systems settings must be applied. This can bring AnyBody to use up to 3GB of memory; however, it is recommended switching to a Win 64 bit system in such cases. This will extend the limit to 4GB.

Some license related problems and solutions
---

#### License Status: "Bad signature in license (-5)" 
The license file has been violated, e.g. manually edited, and is no longer valid. A new license file is required.

#### License Status: "No license check or No authorization for product or License server doesn’t support this. Make sure you have imported a valid license file."
(a) No valid license file has been imported on the AnyBodyTM client computer.
(b) The product name for the license is wrong. A new license file is required.
(a) No license file has been imported in the client computers. Please refer to License status below of this guide.

#### License Status: "All licenses in use (-22)"
(a) Too many license instances have been checked out. Each instance of AnyBodyTM count as one license check-out, even two instances on the same computer. This is related to counted licenses only.

#### License Status: "Wrong host for license (-4)"
(a) The license file is used by the wrong host, i.e., on the wrong computer.
(b) License is linked to a hardware dongle, which is missing or not accepted on the computer (drivers missing).
(c) Wrong dongle is used.

####  License Status: "Communication with server failed! Please make sure the server has been started."

(a) Wrong server hostname specified in the client license file. This can be changed without
violating the license file signature.
(b) Different TCP/IP port number specified for license server and in client license file. The
port number can be modified in the license file without violating its signature. Notice
that from RLM v6, the default port is 5053 instead of 28000.
(c) Network issues. Make sure the client can contact the server properly. It may be neces-
sary to set up firewall exceptions on the server and/or the client computer.
(d) License server is not accepting your license. There are three typical reasons for this,
please refer to Section 4.1:
i. License server is not started.
ii. The license file is missing on the server.
iii. The AnyBodyTM Technology definition file (a file identifying AnyBody Technology
to the generic RLM server software) is missing in the license server setup


#### How to make the license work for FE converters 
Once the converter has been installed please copy the license file into the same directory as the tool is installed into and ensure that the file is named "license.lic"

Remember to ensure that the license is valid for the PC running the converter and that license has valid maintenance.

Typical Problems
---

#### It is impossible to contact the server from the client althought the server is available from the network, with service started: 
If the RLM License Server computer is visible over the network from the client machine, but AnyBody is unable to contact the RLM License Server, there are a couple of likely causes:

1: The license server computer is running a firewall, and RLM has not been added to the firewall exceptions. Depending on the Windows version on the license server computer, you can either add the RLM executable to the firewall exceptions or set up RLM to use specific port numbers and add these ports to the exceptions.

2: The license files on the client computer might not have the correct names for the license servers in them, but I think we created them with the specific server-names for UTBM, so I think this option is less likely.

3: The RLM License Server programs on the license server computers might not be correctly serving the AnyBody license. You can test that the RLM service is providing the floating licenses by opening a command prompt on the primary license server computer and navigating to the RLM folder, and in this folder type the following command:

“rlmutil rlmstat –a”. This should provide you with details about which licenses are currently being served by the RLM License Server.

It would be helpful if they could open the “AnyBody License Manager” window (from the Help->Registration menu item) in AnyBody on the client machine, press the “View License Status” button, and copy me the contents of the resulting dialog.

#### Which port should be used when a 2nd RLM service already uses port 5054 on the same server?: 

The recommended method for using a single license server computer to serve licenses from more than one ISV is to only run a single RLM service on the computer, and let it run more than one ISV server. In this case it means that it would have both the “anybodytch.set” and one or more other ISV settings files along with all the licenses that it is serving. When used that way it should not be necessary to specify a specific port for the RLM or ISV servers.

