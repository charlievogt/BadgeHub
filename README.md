# Red Tag Printer for Troubled Assets

The **Red Tag Printer for Troubled Assets** prints out red tags which contain the assets's number and trouble issue, technician name in plain text, and the RED TAG wordage. The information is recorded in a `.csv` file and automatically uploaded to Google sheets when the Sheets API is turned on. 



Pre-requisites
--------------
* Raspberry Pi 2 or 3 running Raspbian in desktop mode.  
* DYMO LabelWriter 450 printer connected to the Raspberry Pi via USB.
* Internet access for the Raspberry Pi.

The Bill of Materials provides the complete list of equipments, pricing, and links to recommended products.

<details><summary>**Bill of Materials (click to open)**</summary><p>

| Item No. | Description | Quantity | Price | Link |
|----------|---------------|----------|-------|------|
| 1 | Raspberry Pi 3 Model B | 1 | $38.31 | [Amazon link](https://www.amazon.com/Raspberry-Pi-RASPBERRYPI3-MODB-1GB-Model-Motherboard/dp/B01CD5VC92) |
| 2 | Raspberry Pi 7" Touchscreen Display | 1 | $66.99 | [Amazon link](https://www.amazon.com/Raspberry-Pi-7-Touchscreen-Display/dp/B0153R2A9I/) |
| 3 | Power Adapter | 1| $9.99| [Amazon link](https://www.amazon.com/CanaKit-Raspberry-Supply-Adapter-Charger/dp/B00MARDJZ4/) |
| 4 | Keyboard | 1 | $14.99| [Amazon link](https://www.amazon.com/Anker-Bluetooth-Ultra-Slim-Keyboard-Devices/dp/B005ONMDYE/) |
| 5 | Micro SD Card | 1| $15.95| [Amazon link](https://www.amazon.com/Samsung-Class-Micro-Adapter-MB-MC32DA/dp/B00WR4IJBE/) |
| 6 | DYMO LabelWriter 450 | 1| $66.95| [Amazon link](https://www.amazon.com/DYMO-LabelWriter-Thermal-Printer-1752264/dp/B0027JBLV4) |
| 7 | DYMO 2-1/4" x 4" labels (30857) | 1 | $10.00 | [Amazon link](https://www.amazon.com/DYMO-Adhesive-LabelWriter-Printers-30857/dp/B00009WO6F) |

**Total Cost:** $223.18

</p></details>

Setting up your Raspberry Pi as a red tag kiosk
-------------------------------------------------
Setting up your Raspberry Pi as a name tag kiosk involves two steps:

1. [Installing the login system](#installing_login)
2. [Adding the DYMO LabelWriter 450 printer](#adding_printer)

Optional step:
-------------
3. [Uploading to Google sheets](#uploading_data)


### <a name="installing_login">Installing the login system</a>

1. Open a terminal and create a new folder called "GitHub". Run: `mkdir ~/GitHub`
    
    **NOTE:** Ensure that the folder name is "GitHub" since the folder name is referenced in the install script. 
2. Clone the git repository from GitHub. Run:
    
    ```
    cd ~/GitHub
    git clone https://github.com/charlievogt/RedTag-System.git
    cd CFSJ-Login-System/
    ```
3. Run the install script: `./install/install.sh`

The installation process may take several minutes to complete. The script installs all the dependencies required for the program including the driver for the DYMO LabelWriter 450 printer. The script also configures the Raspberry Pi to start Chromium automatically on startup.

### <a name="adding_printer">Adding the DYMO LabelWriter 450 printer</a>

1. Open Chromium and browse to [http://localhost:631/](http://localhost:631/).
2. Click the **Administration** tab at the top and click **Add Printer** under Printers.
3. In the **Authentication Required** dialog box, enter `pi` as the user name and `raspberry` as the password.
4. Click **Log In**.
5. In the Add Printer page, select **DYMO LabelWriter 450 (DYMO LabelWriter 450)** and click **Continue**.
6. Review the Name and Description and click **Continue**.
7. Select **DYMO LabelWriter 450 (en)** from the Model list.
8. Click **Add Printer**.
9. In the Set Default Options for DYMO_LabelWriter_450 page, set the following:
	* Media size: **30857 Badge Label**
	* Output Resolution: **300x600 DPI**
	* Halftoning: **Error Diffusion**
	* Print Density: **Normal**
	* Print Quality: **Barcodes and Graphics**
10. Click **Set Default Options**. You will be redirected to the Printers tab.
11. Click the **Administration** drop-down and select **Set As Server Default**.
12. Finally, close the browser and reboot the device.

After rebooting, the Raspberry Pi will automatically start Chromium in kiosk mode and display the login system.


### <a name="uploading_data">Uploading to Google sheets</a>

1. Open Chromium and browse to [https://developers.google.com/sheets/api/quickstart/python](https://developers.google.com/sheets/api/quickstart/python).
2. Follow the instructions under "Step 1: Turn on the Google Sheets API" to create and download client_secret.json.
3. Copy the file into the RedTag_System folder.

The uploder.py copies the info from the `.csv` file into Google sheets. After a successful update, the `.csv` file is deleted. If the update fails, the user information is retained in the `.csv` file until a successful retry.

